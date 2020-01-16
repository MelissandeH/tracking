# Titre du projet

Module File Upload , page Tracking Log System

## Introduction

Nous allons voir ci-dessous comment utiliser et configurer le module d'envoi de fichier sur la page Tracking.

### Fichiers 

- _edittracking.php (index de la page)
- _delete.php
- _file-upload.php
- _update.php
- _updateCategory.php

### Pré-requis

Sont inclus dans ce module :

```
Bootstrap
```

```
DropZoneJS
```

```
Jquery
```

```
Fontawesome
```

## Utiliser le module d'envoi de fichiers

Il existe deux façons d'envoyer des fichiers via le module d'envoi de fichier

A) Envoyer un seul fichier : 

1. Cliquez sur "Définir une catégorie"
2. Puis sur "Parcourir" à fin d'envoyer votre fichier
3. Son nom est donc directement indiqué dans "Votre fichier" , vous pouvez le renommer
4. Si vous le souhaitez, vous avez la possibilité d'ajouter un commentaire lié à votre document
5. Pour finir, choisissez la catégorie associée à votre fichier
6. Appuyez sur "Envoyer".

B) Envoyer plusieurs fichiers à la fois :

{En cours de développement}

### Personnalisation disponibles

- id (GET)
- Nom du fichier (POST)
- Catégorie (AJAX)
- Commentaire (AJAX)

### Actions disponibles

- Visualiser (HREF)
- Télécharger (HREF)
- Supprimer (AJAX)

## Développement

###### 186 à 238 : Multiple file upload (+btn file upload pour un fichier)
{Développement en cours}
Le bouton "Définir une catégorie" fait apparaître une fenêtre 'pop-up' permettant de personnaliser son envoi
    
 ```php
 <div class="container nopadding mb-3" id="files">
                    <h5 class="py-2 mb-3 title">Fichiers</h5>
                    <div class="dropzone-section">
                        <div class="content-card px-4 py-2">
                            <div class="row align-items-center">
                                <div class="col-md-12">
                                    <!-- Uploaded files section -->
                                    <h4 class="section-sub-title"><span>Uploaded</span> Files (<span class="uploaded-files-count">0</span>)</h4>
                                    <span class="no-files-uploaded ">No files uploaded yet.</span>
                                    <!-- Preview collection of uploaded documents -->
                                    <div class="preview-container dz-preview uploaded-files">
                                        <div id="previews">
                                            <div id="onyx-dropzone-template">
                                                <div class="onyx-dropzone-info">
                                                    <div class="thumb-container">
                                                        <img data-dz-thumbnail />
                                                    </div>
                                                    <div class="details">
                                                        <div>
                                                            <span data-dz-name></span> <span data-dz-size></span>
                                                        </div>
                                                        <div class="dz-progress"><span class="dz-upload" data-dz-uploadprogress></span></div>
                                                        <div class="dz-error-message"><span data-dz-errormessage></span></div>
                                                        <div class="actions">
                                                            <a href="#!" data-dz-remove><i class="fa fa-times"></i></a>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-12 my-3 text-center">
                                    <!-- Files section MULTIPLE FILE-->
                                    <div class="dropzone-file p-5 mb-2 ">
                                        <h4 class="section-sub-title"><span>Upload</span> Your Files</h4>
                                        <form action="file-upload.php?<?php echo "id=" . $result->getResult(0) ?>" class="dropzone files-container ">
                                            <div class="fallback">
                                                <input name="myfile" type="file" multiple />
                                            </div>
                                        </form>
                                    </div>
                                    <!-- Notes -->
                                    <span style="font-size:9px;">Only JPG, PNG, PDF, DOC (Word), XLS (Excel), PPT, ODT and RTF files types are supported.</span>
                                    <span style="font-size:9px;">Maximum file size is 25MB.</span>
                                    <div class="footer-card py-2" style="background-color:transparent;">
                                        <button type="button" class="btn btn-outline-success" data-toggle="modal" data-target="#exampleModalCenter" style="width:100%">Définir une catégorie</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
```

###### 240 à 317 : Table secondaire générée local** (uploads/docs)
On génère une table grâce à une boucle répertoriant tous les fichiers se trouvant dans '/var/www/web/uploads/docs'
Deux actions sont associées à ces fichiers : Visualiser et Enregistrer
    
```php
                <div class="container mb-3 py-4 nopadding" id="infos">
                    <div class="header mb-0">
                        <?php $dir = "/var/www/web/uploads/docs"; ?>
                        <h5 class="py-1">Files List from <?php echo $dir ?></h5>
                    </div>
                    <section style="height:500px;overflow:auto;">
                        <?php
                        $filename = array_filter(scandir('/var/www/web/uploads/docs'), function ($item) {
                            return !is_dir('/var/www/web/uploads/docs/' . $item); // On retire les folders (dir) pour display uniquement les files
                        });
                        $extensionName = substr($filename, strpos($filename, ".") + 1);
                        echo "<table class=\"datatables tablesorter sizableColumns searchResults\" >";
                        echo "<thead ><tr><th class=\"text-center\" style=\"width:40px;\">Type</th><th class=\"text-center\">Nom du fichier</th><th class=\"text-center\">Action</th></tr></thead>";
                        echo "<tbody>";
                        foreach ($filename as $file) {
                        ?>
                            <tr>
                                <td>
                                    <?php $extension = pathinfo($dir . '/' . $file);
                                    // Génère : <body text='black'>
                                    $extension['extension'] = str_replace(array("png", "jpg"), array("image"), $extension['extension']);
                                    ?>
                                    <span class="fa fa-file-<?php echo $extension['extension'] ?>-o" style="font-size:36px"></span>
                                </td>
                                <td class="text-left" style="padding:10px;">
                                    <h4 class="table-title"><?php echo $file ?></h4>
                                    <span>
                                        <?php
                                        echo "<b>dirname</b> : " . $extension['dirname'], "\n <br/>";
                                        echo "<b>basename</b> : " . $extension['basename'], "\n <br/>";
                                        echo "<b>extension</b> : " . $extension['extension'], "\n <br/>";
                                        echo "<b>filename</b> : " . $extension['filename'], "\n <br/>"; // since PHP 5.2.0 
                                        ?>
                                    </span>
                                </td>

                                <?php
                                //Script pour se connecter à la BDD, récuperer les edocuments et génerer une boucle pour les afficher//
                                try {
                                    $connection = Doctrine_Manager::connection();
                                } catch (Exception $e) {
                                    exit('Erreur : ' . $e->getMessage());
                                }
                                $reponse_edoc = $connection->prepare("SELECT * FROM etracing.edoc ORDER BY code");
                                $reponse_edoc->execute(array(
                                    'iore' => 1,
                                    'eflow_id' => $flow->getId()
                                ));
                                $donnees_evaluedoc = $reponse_edoc->fetchAll();
                                foreach ($donnees_evaluedoc as $row) {
                                    // On génère les catégories ainsi que les classes personnalisées//
                                }
                                ?>

                                <?php
                                $evalue_id = $result->getResult(0);
                                $reponse_evaluedoc = $connection->prepare("SELECT * FROM etracing.evaluedoc WHERE evalue_id=$evalue_id");
                                $reponse_evaluedoc->execute(array(
                                    'id' => $flow->getId()
                                ));
                                $donnees_evaluedoc = $reponse_evaluedoc->fetchAll();
                                // On génère les catégories ainsi que les classes personnalisées//
                                ?>

                                <td>
                                    <span><a href=" <?php echo "../uploads/docs/" . $extension['basename'] ?>" target="_blank">
                                        <i class="fas fa-eye" style="color:green;padding:5px;"></i></a>
                                    </span>
                                    <span><a download="" href="../uploads/docs/<?php echo $extension['basename']; ?>">
                                        <i class="fas fa-download" style="padding:5px;"></i></a></span>
                                </td>
                            </tr>
                        <?php }
                        echo "</tbody>
                    </table>" ?>
                    </section>
                </div>
```

###### 321 à 480 : Table principale générée BDD (devtracing)
    1. On génère une table grâce à une boucle récupérant le filename, la catégorie, le commentaire qui correspondent à l'id de la session
    2. Le type est créé grâce au filename qui est lui-même décomposé à fin de récuperer l'extension
    3. Trois actions sont disponibles : visualiser et enregistrer mais également supprimer qui permet de retirer les données du document de la BDD {+ Développement en cours : suppresion du fichier en local}
    4. Deux champs sont ainsi personnalisables : catégorie ainsi que commentaire. Le nom du fichier lui est modifiable seulement à sa création dans file upload
    
```php
                <div class="container mb-3 py-4 nopadding" id="infos">
                    <div class="header mb-0">
                        <h5 class="py-1">Files List from Database (etracing.evaluedoc)</h5>
                    </div>
                    <section style="overflow:auto;">
                        <?php
                        echo "<table class=\"datatables tablesorter sizableColumns searchResults\" >";
                        echo "<thead ><tr><th class=\"text-center\" style=\"width:40px;\">Type</th><th class=\"text-center\">Nom du fichier</th><th class=\"text-center\">Catégorie</th><th class=\"text-center\">Commentaire</th><th class=\"text-center\">Action</th></tr></thead>";
                        echo "<tbody>";
                        foreach ($donnees_evaluedoc as $row) {
                        ?>
                            <tr class="record">
                                <td>
                                    <?php $extension = end(explode(".", $row['filename']));
                                    $extension = str_replace("png", "image", $extension);
                                    $extension = str_replace("jpg", "image", $extension); ?>
                                    <span class="fa fa-file-<?php echo $extension ?>-o" style="font-size:36px"></span>
                                </td>
                                <td class="text-left" style="padding:10px;">
                                    <h4 class="table-title"><?php echo $row['filename'] ?></h4>
                                </td>
                                <td>
                                    <div class="form-group">
                                        <label for="exampleFormControlSelect1"></label>
                                        <select class="form-control categoryList" name="selectList" data-id="<?php echo $row['id']; ?>">
                                            <?php
                                            $reponse_edoc->execute(array(
                                                'iore' => 1,
                                                'eflow_id' => $flow->getId()
                                            ));
                                            $donnees_edoc = $reponse_edoc->fetchAll();

                                            foreach ($donnees_edoc as $row_edoc) {
                                                // On génère les catégories ainsi que les classes personnalisées//
                                                echo
                                                    "<option data-id = " . $row['id'] . " value=" . $row_edoc['id'] . " for=" . $row_edoc['id'] . '_' . $row_edoc['code'] . "";
                                                if ($row_edoc['id'] == $row['edoc_id']) {
                                                    echo " id=\"selected\" selected ";
                                                }
                                                echo
                                                    ">" . $row_edoc['code'] .  "</option></div>"; ?>
                                            <?php } ?>
                                            <input class="projectId" type="hidden" name="projectId" value="<?php echo $row['id']; ?>" />
                                        </select>
                                    </div>
                                </td>
                                <?php
                                echo '<td contentEditable=\'true\' class=\'editComment\' id=\'comment_' . $row['id'] . '\'>';
                                $donnees_evaluedoc = $reponse_evaluedoc->fetchAll();
                                echo '<h4 class="table-title">' . $row['descr'] . '</h4>';
                                $id = $row['id'];
                                ?>
                                </td>
                                <td>
                                    <?php
                                    foreach ($donnees_evaluedoc as $row) {
                                        var_dump($row['filename']);
                                        echo '<br/>';
                                    } ?>
                                    <span><a href=" <?php echo "../uploads/docs/" . $row['filename'] ?>" target="_blank"><i class="fas fa-eye" style="color:green;padding:5px;"></i></a>
                                    </span>
                                    <span><a download="" href="../uploads/docs/<?php echo $row['filename']; ?>"><i class="fas fa-download" style="padding:5px;"></i></a></span>
                                    <span><a id="<?php echo $row['id']; ?>" class="delbutton"><i class="fas fa-times" style="color:red;padding:5px;"></i></a></span>
                                </td>
                            </tr>
                        <?php }
                        echo "</tbody>
                    </table>" ?>
                        <!-- Delete TR (= 1 file) table -->
                        <script>
                            $(function() {
                                $(".delbutton").click(function() {
                                    var del_id = $(this).attr("id");
                                    var info = del_id;
                                    if (confirm("Sure you want to delete this post? This cannot be undone later.")) {
                                        $.ajax({
                                            type: "POST",
                                            url: "/apps/frontend/modules/renderersDetails/templates/_delete.php", //URL to the delete php script
                                            data: {
                                                id: info
                                            },
                                            success: function() {
                                                console.log(info)
                                            }
                                        });
                                        $(this).parents(".record").animate("fast").animate({
                                            opacity: "hide"
                                        }, "slow");
                                    } else {
                                        return false;
                                    }
                                });
                            });
                        </script>

                        <!-- Catégories editable -->
                        <script>
                            $(document).ready(function() {
                                $('.categoryList').change(function() {
                                    $.ajax({
                                        type: 'POST',
                                        url: '/apps/frontend/modules/renderersDetails/templates/_updateCategory.php',
                                        data: {
                                            code_id: $("option:selected", this).val(),
                                            id: $(this).find(':selected').data('id')
                                        },
                                        success: function(response) {
                                            console.log(response);
                                            console.log('Save successfully');
                                        },
                                        error: function() {
                                            console.log(xhr, ajaxOptions, thrownError);
                                        },
                                        complete: function() {
                                            console.log('complete');
                                        }
                                    });
                                });
                            });
                        </script>

                        <!-- Commentaire editable -->
                        <script>
                            $(document).ready(function() {
                                // Add Class
                                $('.editComment').click(function() {
                                    $(this).addClass('editMode');
                                });
                                // Save data
                                $(".editComment").focusout(function() {
                                    $(this).removeClass("editMode");
                                    var id = this.id;
                                    var split_id = id.split("_");
                                    var field_name = split_id[0];
                                    var edit_id = split_id[1];
                                    var value = $(this).text();
                                    $.ajax({
                                        url: '/apps/frontend/modules/renderersDetails/templates/_update.php',
                                        type: 'POST',
                                        data: {
                                            field: field_name,
                                            value: value,
                                            id: edit_id
                                        },
                                        success: function(data) {
                                            console.log(data);
                                            console.log('Save successfully');
                                        },
                                        error: function() {
                                            console.log(xhr, ajaxOptions, thrownError);
                                        },
                                        complete: function() {
                                            console.log('complete');
                                        }
                                    });
                                });
                            });
                        </script>
                    </section>
                </div>
```

###### 488 à 636 : Fenêtre pop-up (déclenchée par le btn "Définir une catégorie)
    1. On upload un fichier que l'on peut tout de suite renommer et lui ajouter une description. 
    2. Si le fichier est un fichier image lisible (PNG,JPG...) un preview apparaîtra entre le titre et la description du fichier envoyé
    3. Il est nécessaire pour l'utilisateur de sélectionner une catégorie à associer à ce document
    4. Les données du documents sont résumées dans le footer de la fenêtre à fin de faciliter la démarche
    
```php
<div class="modal fade" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content" style="  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.2);">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLongTitle"><i class="fas fa-file-signature"></i> Envoyer un fichier</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <form id="myform" action="/apps/frontend/modules/renderersDetails/templates/_file-upload.php?<?php echo "id=" . $result->getResult(0) ?>" method="POST" enctype="multipart/form-data">
                <div class="modal-body">
                    <div class="col-md-12">
                        <div class="row my-4">
                            <h3 class="text-left mb-2" style="width:100%; padding: 5px; border: 1px solid #ccc; border-radius:4px;">
                                <input onchange="changeValue()" type="text" id="searchTxt" value="" maxlength="30" placeholder="Votre fichier" name="activename" style="width: 94%;border:0px solid white;"></h3>
                            <img id="imageResult" />

                            <div class="md-form" style="width:100%;">
                                <textarea name="description" id="materialContactFormMessage" class="form-control md-textarea  my-2" placeholder="Ajouter un commentaire" style="margin:0; padding:10px;"></textarea>
                            </div>
                            <script>
                                function myFunction() {
                                    document.getElementById("searchTxt").value = document.getElementById('fileName').innerHTML;
                                }
                            </script>
                            <input type="file" name="image" id="image" class=" p-0 m-0" />

                            <script>
                                /** Script pour display l'upload file name dans l'input, JS */
                                $(document).ready(function() {
                                    $('input[type="file"]').change(function(e) {
                                        var fileName = e.target.files[0].name;
                                        $('input[name=activename]').val(fileName);
                                        $("#searchValue").html(fileName);;
                                    });
                                });
                            </script>

                            <script>
                                // DISPLAY PREVIEW IMAGE UPLOAD
                                document.getElementById("image").onchange = function() {
                                    var reader = new FileReader();
                                    reader.onload = function(e) {
                                        // get loaded data and render thumbnail.
                                        document.getElementById("imageResult").src = e.target.result;
                                    };
                                    // read the image file as a data URL.
                                    reader.readAsDataURL(this.files[0]);
                                };
                            </script>

                            <script>
                                // DISPLAY FILENAME ON CHANGE
                                function changeValue() {
                                    var nameValue = $("#searchTxt").val();
                                    // When using jQuery 3:
                                    // var multipleValues = $( "#multiple" ).val();
                                    $('h4#searchValue').text(nameValue);
                                }
                            </script>
                        </div>
                    </div>

                    <div class="col-md-12">
                        <div class="row">
                            <h3 class="text-left">Files Category (1 max)</h3>
                            <div class="container-fluid py-4">
                                <div class="row align-items-center h-100">
                                    <div class="cc-selector-2">

                                        <?php
                                        //Script pour se connecter à la BDD, récuperer les edocuments et génerer une boucle pour les afficher//

                                        if (!empty($_FILES)) {
                                            $targetDir = "/web/uploads/docs";
                                            $filename = $_FILES['file']['name'];
                                            $targetFilePath = $targetDir . $filename;

                                            if (move_uploaded_file($_FILES['file']['tmp_name'], $targetFilePath)) {
                                                $insert = $db->query("INSERT INTO evaluedoc ( file_name, uploaded) VALUES ( $fileName, '" . date("Y-m-d H:i:s") . "')");
                                            }
                                        }

                                        $reponse = $connection->prepare("SELECT id, name, code FROM etracing.edoc");
                                        $reponse->execute(array(
                                            'iore' => 1,
                                            'eflow_id' => $flow->getId()
                                        ));
                                        $donnees_evaluedoc = $reponse->fetchAll();
                                        foreach ($donnees_evaluedoc as $row) {
                                            // On génère les catégories ainsi que les classes personnalisées//
                                            echo "<div class=\"col-md-3 box-one\">";
                                            echo
                                                "<input type=\"radio\" name=\"category\" id=" . $row['id'] . '_' . $row['code'] . " value=" . $row['id'] . ">";
                                            echo
                                                "<label class=\"category-card category\" for=" . $row['id'] . '_' . $row['code'] . ">" . $row['code'] . "</label>";
                                            echo "</div>";
                                        }
                                        ?>

                                    </div>
                                    <div class="container-fluid">
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer pt-4">
                    <div class="row" style="width:100%;">
                        <div class="col-sm-12 text-left">
                            <span class="modal-footer-infos">
                                <?php
                                echo "<h3 >Votre fichier</h3> ";
                                echo "<h4 id=\"searchValue\"></h4>";
                                ?>
                            </span>
                        </div>
                        <div class="col-sm-12 text-left">
                            <span class="modal-footer-infos">
                                <?php
                                echo "<h3>Catégorie sélectionnée : </h3> ";
                                echo "<h4 id=\"resultcategory\"> </h4>";
                                ?>
                            </span>
                        </div>
                    </div>
                    <button type="submit" name="submit" class="btn btn-primary">Envoyer</button>
                </div>
        </div>
        </form>

        <script>
            /*Récuperer radio selected pour l'afficher*/
            $('#myform input[type=radio]').on('change', function(event) {
                var result = $(this).val();
                $('#resultcategory').html(result);
            })
        </script>

        <script>
            /*Script pour ajouter la classe selected au clic*/
            $('.category').on('click', function() {
                $('.category').removeClass('selected');
                $(this).addClass('selected');
            });
        </script>
    </div>
</div>
```
