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
// use PHPUnit\Framework\TestCase;



class EncryTestController extends Controller
{

public function index(){

  $user =4;

  try {
      // First, manage the keys
      if (!file_exists("$user-secret-key.txt")) {
          $secretKey = KeyFactory::generateEncryptionKey();
          KeyFactory::save($secretKey, "$user-secret-key.txt");
      } else {
          $secretKey = KeyFactory::loadEncryptionKey("$user-secret-key.txt");
      }
      $message = new HiddenString('correct horse battery staple');
      $ciphertext = Symmetric::encrypt($message, $secretKey);

      if ($ciphertext) {
          echo 'Access granted', "\n";
          echo $ciphertext;



      } else {


          echo 'Access DENIED!', "\n";
          exit(255);
      }
  } catch (Throwable $ex) {
      echo $ex->getMessage(), PHP_EOL;
      echo $ex->getTraceAsString(), PHP_EOL;
      exit(127);
  }

//  echo $raw_ciphertext;exit;


}

  public function val(){

    /**
    * Generate a new encryption key
    */
    try {
        // First, manage the keys
        if (!file_exists('01-secret-key.txt')) {
            $secretKey = KeyFactory::generateEncryptionKey();
            KeyFactory::save($secretKey, '01-secret-key.txt');
        } else {
            $secretKey = KeyFactory::loadEncryptionKey('01-secret-key.txt');
        }
        $password = new HiddenString('correct horse battery staple');
        $hash = Password::hash($password, $secretKey);
        if (Password::verify($password, $hash, $secretKey)) {
            echo 'Access granted', "\n";



        } else {


            echo 'Access DENIED!', "\n";
            exit(255);
        }
    } catch (Throwable $ex) {
        echo $ex->getMessage(), PHP_EOL;
        echo $ex->getTraceAsString(), PHP_EOL;
        exit(127);
    }

    echo $raw_ciphertext;exit;

  }

public function test(){
    $user = 3;
        try {
$secretKey = KeyFactory::loadEncryptionKey("$user-secret-key.txt");
$ciphertext="MUIEADd-rAIzhhAl26f2ulu4VpnhC-atwKRPWmNWP3Pv8YpPABpscYQvOsDBW6FUQPjWsqv28XnnbV_agwLYVVBi9LLPfgMDCD6qjnFKOPFYGs1cZ76lQXbKqkx7SiulonJsKRO2WZEf5KONPLWFTYnb8fQndM89hOVOYZ7qpyJrDmHuUcdc_6qnf87G5d0r_OTWNstGPUQ=";
$decrypted = Symmetric::decrypt($ciphertext, $secretKey);

 //print_r($ciphertext);s

 print_r($decrypted-> getString());
}
 catch (\ParagonIE\Halite\Alerts\HaliteAlert $e) {
   // Oh no!
     echo 'Access DENIED!', "\n";

}


//var_dump($decrypted->getString() === $message->getString()); // bool(true)



}

public function asy_encrypt(){


$aliceSecretKey = KeyFactory::loadEncryptionSecretKey('/path/to/enc.secret.key');
$BobPublicKey =   KeyFactory::loadEncryptionPublicKey('/path/to/bob.enc.public.key');

// And now, to encrypt a message:
$encrypted = Asymmetric::encrypt(
    'this is your plaintext',
    $aliceSecretKey,
    $bobPublicKey
);
}

public function asy_dcrypt(){
  $bobSecretKey = KeyFactory::loadEncryptionSecretKey('/path/to/enc.secret.key');
  $alicePublicKey = KeyFactory::loadEncryptionPublicKey('/path/to/alice.enc.public.key');

// And now, to decrypt a message:
$plaintext = Asymmetric::decrypt(
    $encrypted,
    $bobSecretKey,
    $alicePublicKey
);
}

public function testEncrypt()
    {
        $alice = KeyFactory::generateEncryptionKeyPair();
        $bob = KeyFactory::generateEncryptionKeyPair();

        $message = Asymmetric::encrypt(
            new HiddenString('test message'),
            $alice->getSecretKey(),
            $bob->getPublicKey()
        );

        print_r($message);

        $plain = Asymmetric::decrypt(
            $message,
            $bob->getSecretKey(),
            $alice->getPublicKey()
        );
       $plain->getString();
    }

}
