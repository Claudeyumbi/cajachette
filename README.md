<?php

$error = array();
$success = false;

if (!empty($_POST['submitform'])) {
  // echo "bravo ! <br>";
  // print_r($_POST);
  //
  // echo "<pre>";
  // print_r($_FILES['image']);
  // print_r($_FILES['pdf']);
  // echo "<pre>";

  if($_FILES['image']['error'] > 0){

        $error['image'] = 'Aucun fichier n\'a été téléchargé';

  }else{
      $filename = $_FILES['image']['name']; // le nom du
      $file_size = $_FILES['image']['size'];
      $file_tmp = $_FILES['image']['tmp_name'];// le chemin du fichier
      $file_type = $_FILES['image']['type']; // type, extension

      // taille du fichier
      $sizeMax = 2000000;
      // filessize();

          if($file_size > $sizeMax || filesize($file_tmp) >$sizeMax){
            $error['image'] = "Fichier trop gros (2 Mo max;)!";

            // echo "Cette taille ne convient pas ! Veuillez changer de fichier";
          }
          else{
              // extension
              $validExtension = array('jpg', 'jpeg', 'png');

              $fileExtension = pathinfo($filename, PATHINFO_EXTENSION);

              if (!in_array($fileExtension, $validExtension)) {
                 $error['image'] = 'Veuillez télécharger une image du type jpg, jpeg ou png';
              }else{
                $goodExtension = array('image/jpg', 'image/jpeg', 'image/png');
                $finfo = finfo_open(FILEINFO_MIME_TYPE);
                $mime = finfo_file($finfo, $file_tmp);
                finfo_close($finfo);

                if(!in_array($mime, $goodExtension)){
                   $error['image'] = 'Veuillez télécharger une image de type jpg, jpeg, png !';

                }else{
                    //   echo "Bravo !";
                    // if(is_dir("image")){
                    //   mkdir("image");
                    //
                    // }
                   $dest_fichier = date('y_m_d_H_i').'_test.'.$fileExtension;

              if (move_uploaded_file($file_tmp, 'public/images/' .$dest_fichier)){
                $success = true;
              }

            }
          }

        }
        // echo "<pre>";
        // print_r($error);
        // echo "<pre>";
        header('Location: account.php');
    }

}

 ?>

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>upload</title>
  </head>
  <body>

      <form action="" class="monImage" enctype="multipart/form-data"
       method="post">

        <input type="file" name="image">
        <!-- <input type="file" name="pdf"> -->
        <input type="submit" name="submitfile" value="envoyer">
      </form>
  </body>
</html>
