<?php
declare(strict_types=1);
namespace App\Http\Controllers;
use Illuminate\Support\Facades\Auth;
use Illuminate\Http\Request;
use App\Keypairs;
use App\Asset;
use App\Type;
use App\Http\Resources\Keypair as KeypairResource;
use DB;
use ParagonIE\Halite\KeyFactory;
use ParagonIE\Halite\HiddenString;
use ParagonIE\Halite\Symmetric\Crypto as Symmetric;
use ParagonIE\Halite\Password;
use ParagonIE\Halite\Halite;
use ParagonIE\Halite\Asymmetric\{
    Crypto as Asymmetric,
    EncryptionPublicKey,
    EncryptionSecretKey
};
use ParagonIE\Halite\Alerts as CryptoException;




class KeypairsController extends Controller
{
      public $sucessStatus = 200;
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
     public function index()
     {
         //get Keypairs
          $user = Auth::user();
          $keypair= DB::table('keypairs')
         ->where('keypairs.user_id','=',$user->id)
         ->get();

         return response()->json(['success'=>$keypair],$this->sucessStatus);
     }

     /**
      * Store a newly created resource in storage.
      *
      * @param  \Illuminate\Http\Request  $request
      * @return \Illuminate\Http\Response
      */
     public function store(Request $request)
     {

         $array = $request->all();
       foreach($array["data"] as $row)
          {

            $heading_id = array();
            $keypair_name = array();
            $keypair_value = array();
            $user = Auth::user();
            $user_id=$user->id;
            $key_value=$row["keypair_value"];
            $e_data = $this->encrypt($keyvalue);
          //insertion of keypair data

                                  $keypair =   keypairs::create([
                                 'heading_id'     => $row["heading_id"],
                                 'keypair_name'   => $row["keypair_name"],
                                 'user_id'        => $user_id,
                                 'keypair_value'  =>$e_data ,

             ]);

               }

         if($keypair->save()){
           return response()->json(['success'=>$keypair],$this->sucessStatus);

         }


     }

     /**
      * Display the specified resource.
      *
      * @param  int  $id
      * @return \Illuminate\Http\Response
      */
     public function show(Request $request)
     {
         //Get single keypair
         $id           =  null;
         $heading_id   =  null;
         $user_id      =  null;

         $id           =  $request->input('id');
         $heading_id   =  $request->input('heading_id');
         $user_id      =  $request->input('user_id');
         $user         = Auth::user();

if($heading_id==null&&$user_id==null){

  $key_pair   = Keypairs::findorfail($id);

}elseif ($id==null&&$user_id==null) {

    $key_pair   = Keypairs::where('heading_id',$heading_id)->where('user_id',$user->id)->get();

}elseif ($id==null&&$heading_id==null) {

$key_pair   = Keypairs::where('user_id',$user_id)->get();

}elseif ($id==null) {
  // code...
  $key_pair   = Keypairs::where('heading_id',$heading_id)->where('user_id',$user_id)->get();
}

         return response()->json(['success'=>$key_pair],$this->sucessStatus);
     }

     /**
      * Update the specified resource in storage.
      *
      * @param  \Illuminate\Http\Request  $request
      * @param  int  $id
      * @return \Illuminate\Http\Response
      */
     public function update(Request $request)
     {
       $id=$request->input('id');

       $messege="Updated";

       //$keypair_name check
       $keypair_name=$request->input('keypair_name');
       if($keypair_name==""){

       }
       else{
       $key = Keypairs::where('id', $id)->update(array('keypair_name' =>$keypair_name));
       }

       $keypair_value=$request->input('keypair_value');
       if($keypair_value==""){

       }
       else{
       $key = Keypairs::where('id', $id)->update(array('keypair_value' =>$keypair_value));
       }


       //user_id check
       $user_id=$request->input('user_id');
       if($user_id==""){

       }
       else{
         // $client->user_id = $request->input('user_id');
         $key = Keypairs::where('id', $id)->update(array('user_id' =>$user_id));
       }

       //item_id check
       $heading=$request->input('heading_id');
       if($heading==""){

       }
       else{
         // $client->user_id = $request->input('user_id');
         $key = Keypairs::where('id', $id)->update(array('heading_id' =>$heading));
       }
        return response()->json(['success'=>$messege],$this->sucessStatus);
     }

     /**
      * Remove the specified resource from storage.
      *
      * @param  int  $id
      * @return \Illuminate\Http\Response
      */



     public function destroy(Request $request)
{
    $id=$request->input('id');
    $user = Auth::user();
    $user_id = $user->id;


  $keypair = Keypairs::where('id', $id)->where('user_id', $user_id)->firstOrFail();
  if($keypair->delete()){
  return response()->json(['success' => $keypair], $this->sucessStatus);

  }
}

public function encrypt($key_value){
  $user =4;

  try {
      // First, manage the keys
      if (!file_exists("$user-secret-key.txt")) {
          $secretKey = KeyFactory::generateEncryptionKey();
          KeyFactory::save($secretKey, "$user-secret-key.txt");
      } else {
          $secretKey = KeyFactory::loadEncryptionKey("$user-secret-key.txt");
      }
      $data = new HiddenString($key_value);
      $ciphertext = Symmetric::encrypt($data, $secretKey);

  } catch (Throwable $ex) {
      echo $ex->getMessage(), PHP_EOL;
      echo $ex->getTraceAsString(), PHP_EOL;
      exit(127);
  }
return $ciphertext;

}


}
