////////////////////////////////////////////////
Comprimir carpetas y archivos Aqui.
try
        {
            using (Archive archive = new Archive(archivoZip))
            {
                foreach (var entry in archive.Entries)
                {
                    if (entry.IsDirectory)
                    {
                        // Si el archivo dentro del ZIP es un directorio, puedes crear la carpeta correspondiente
                        string carpetaDestinoCompleta = Path.Combine(Path.GetDirectoryName(archivoZip), entry.Name);
                        Directory.CreateDirectory(carpetaDestinoCompleta);
                    }
                    else
                    {
                        // Si el archivo dentro del ZIP es un archivo, extraerlo
                        string rutaArchivoDestino = Path.Combine(Path.GetDirectoryName(archivoZip), entry.Name);

                        if (!File.Exists(rutaArchivoDestino))
                        {
                            entry.Extract(rutaArchivoDestino);
                            Console.WriteLine($"Archivo extraído: {rutaArchivoDestino}");
                        }
                        else
                        {
                            Console.WriteLine($"El archivo ya existe: {rutaArchivoDestino}");
                        }
                    }
                }
            }
            Console.WriteLine("Proceso completado.");
        }
        catch (Exception)
        {
            throw;
        }
//////////////////////////////////////////////////////////////
Etraer un archivo zip (Extraer en)

        // Ruta del archivo ZIP que quieres extraer
        string archivoZip = @"C:\Users\ricky\Desktop\ArchivosTest/ArchivosTest.zip";

        // Directorio de destino donde se extraerán los archivos
        string directorioDestino = @"C:\Users\ricky\Desktop\ArchivosTest";


        // Crear una instancia de la clase ZipArchive
        using (var zipArchive = new Archive(archivoZip))
        {
            // Obtiene el nombre del archivo ZIP sin la extensión
            string nombreCarpeta = Path.GetFileNameWithoutExtension(archivoZip);

            // Crea la carpeta de destino si no existe
            string carpetaDestino = Path.Combine(directorioDestino, nombreCarpeta);
            Directory.CreateDirectory(carpetaDestino);

            // Extrae los archivos del ZIP en la carpeta de destino
            zipArchive.ExtractToDirectory(carpetaDestino);
        }
  //////////////////////////////////////////////////

Comprimir archivo con o sin ruta 
 // Verificar si la ruta de destino está en blanco
        if (string.IsNullOrWhiteSpace(rutaDestino))
        {
            // Si la ruta de destino está en blanco, utiliza el nombre del archivo como nombre de destino
            rutaDestino = Path.Combine(
                Path.GetDirectoryName(archivoOrigen),
                Path.GetFileNameWithoutExtension(archivoOrigen) + ".zip"
            );
        }
        // Crear FileStream para el archivo ZIP de salida
        using (FileStream zipFile = File.Open(rutaDestino, FileMode.Create))
        {
            // Archivo que se agregará al archivo
            using (FileStream source1 = File.Open(archivoOrigen, FileMode.Open, FileAccess.Read))
            {
                using (var archive = new Archive(new ArchiveEntrySettings()))
                {
                    // Agregar archivo al archivo
                    archive.CreateEntry(archivoOrigen, source1);
                    // archivo zip
                    archive.Save(zipFile);
                }
            }
        }
/////////////////////////////////////////////////////////////
Comprimir Usnado listas

using (Archive zipArchive = new Archive())
        {
            // Lista de archivos que deseas comprimir
            List<string> archivosAComprimir = new List<string>
            {
                @"C:\Users\ricky\Desktop\ArchivosTest\ArchivoTest2.docx",
                @"C:\Users\ricky\Desktop\ArchivosTest\ArchivoTest.docx"
            };

            // Agregar archivos a la lista de archivos del archivo ZIP
            foreach (string archivo in archivosAComprimir)
            {
                zipArchive.CreateEntry(archivo,archivo);
            }

            // Nombre del archivo ZIP de salida
            string archivoZipSalida = @"C:\Users\ricky\Desktop\ArchivosTest\archivos_comprimidos.zip";

            // Guardar el archivo ZIP comprimido
            zipArchive.Save(archivoZipSalida);
        }

        Console.WriteLine("Archivos comprimidos exitosamente.");

////////////////////////////////////////////////////////////

Comprimir Una carpeta

       if (string.IsNullOrWhiteSpace(archivoDestino))
        {
            archivoDestino = Path.Combine(Path.GetDirectoryName(carpetaOrigen), Path.GetFileNameWithoutExtension(carpetaOrigen) + ".zip");
        }

        // Crear un objeto ZipArchive para comprimir la carpeta
        using (var zipArchive = new Archive())
        {
            // Agregar la carpeta al archivo ZIP
            zipArchive.CreateEntries(carpetaOrigen);

            // Guardar el archivo ZIP en la ruta de salida especificada
            zipArchive.Save(archivoDestino);
        }
////////////////////////////////////////////////////////////////
 List<string> carpetasAComprimir = new List<string>
        {
            @"C:\Users\ricky\Desktop\ArchivosTest\Nueva carpeta",
           @"C:\Users\ricky\Desktop\ArchivosTest\ArchivosTest",
            // Agrega más rutas de carpetas según tus necesidades
        };

        // Ruta del archivo ZIP de salida
        string archivoZipSalida = @"C:\Users\ricky\Desktop\ArchivosTest\resultado.zip";

        // Comprimir las carpetas en un archivo ZIP
        using (var archive = new Archive())
        {
            foreach (string carpeta in carpetasAComprimir)
            {
                // Agrega cada carpeta a la lista de archivos a comprimir
                archive.CreateEntries(carpeta);
            }

            // Guarda el archivo ZIP resultante
            archive.Save(archivoZipSalida);
        }

        Console.WriteLine("Carpetas comprimidas con éxito en " + archivoZipSalida);

        Console.WriteLine($"La carpeta '{carpetaOrigen}' ha sido comprimida en '{archivoDestino}'.");

////////////////////////////////////////////////////////
Comprimir vrias carpetas

 List<string> carpetasAComprimir = new List<string>
        {
            @"C:\Users\ricky\Desktop\ArchivosTest\Nueva carpeta",
           @"C:\Users\ricky\Desktop\ArchivosTest\ArchivosTest",
            // Agrega más rutas de carpetas según tus necesidades
        };

        // Ruta del archivo ZIP de salida
        string archivoZipSalida = @"C:\Users\ricky\Desktop\ArchivosTest\resultado.zip";

        // Comprimir las carpetas en un archivo ZIP
        using (var archive = new Archive())
        {
            foreach (string carpeta in carpetasAComprimir)
            {
                // Agrega cada carpeta a la lista de archivos a comprimir
                archive.CreateEntries(carpeta);
            }

            // Guarda el archivo ZIP resultante
            archive.Save(archivoZipSalida);
        }

        Console.WriteLine("Carpetas comprimidas con éxito en " + archivoZipSalida);

////////////////////////////////////////
        
Program
{
    static void Main()
    {
        // Ruta al archivo ZIP de origen
        string zipFilePath = "ruta/al/archivo.zip";

        // Contraseña esperada para el archivo ZIP
        string expectedPassword = "tu_contraseña_esperada";

        // Obtener la carpeta que contiene el archivo ZIP
        string zipFileFolder = Path.GetDirectoryName(zipFilePath);

        // Crear un objeto ZipArchive para abrir el archivo ZIP con contraseña
        try
        {
            using (var archive = new ZipArchive(zipFilePath))
            {
                // Verificar si el archivo ZIP está protegido con contraseña
                if (archive.EncryptionMethod != EncryptionMethod.None)
                {
                    // Solicitar la contraseña al usuario
                    Console.Write("Ingrese la contraseña para el archivo ZIP: ");
                    string enteredPassword = Console.ReadLine();

                    // Verificar si la contraseña es correcta
                    if (enteredPassword != expectedPassword)
                    {
                        Console.WriteLine("Contraseña incorrecta. No se pueden extraer archivos.");
                        return;
                    }
                }

                // Iterar a través de las entradas del archivo ZIP
                foreach (var entry in archive.Entries)
                {
                    // Verificar si la entrada es un archivo
                    if (!entry.IsDirectory)
                    {
                        // Construir la ruta completa del archivo de destino
                        string destinationFilePath = Path.Combine(zipFileFolder, entry.Name);

                        // Verificar si el archivo de destino ya existe
                        if (File.Exists(destinationFilePath))
                        {
                            Console.WriteLine($"El archivo {entry.Name} ya existe en la carpeta de origen del archivo ZIP.");
                        }
                        else
                        {
                            // Extraer el archivo del archivo ZIP a la carpeta de origen del archivo ZIP
                            entry.Extract(destinationFilePath);
                            Console.WriteLine($"Se extrajo el archivo {entry.Name} en la misma carpeta que contiene el archivo ZIP.");
                        }
                    }
                }
            }

            Console.WriteLine("Proceso completado.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Se produjo una excepción: {ex.Message}");

////////////////


  contraseña
string zipFilePath = "ruta/archivo.zip";
string password = "tu_contraseña";

// Abre el archivo zip con contraseña
using (Archive archive = new Archive())
{
    archive.Encryption = new EncryptionInfo(EncryptionAlgorithm.Aes128Bit, password);
    archive.Open(zipFilePath);

    // Carpeta que deseas extraer
    string folderToExtract = "nombre_de_la_carpeta";

    // Directorio de destino
    string targetDirectory = "directorio_de_destino";

    // Itera a través de los elementos del archivo zip
    foreach (ArchiveEntry entry in archive)
    {
        if (entry.FullName.StartsWith(folderToExtract))
        {
            string entryPath = Path.Combine(targetDirectory, entry.FullName.Substring(folderToExtract.Length));

            // Verifica si el archivo ya existe en el destino
            if (File.Exists(entryPath))
            {
                // El archivo ya existe, puedes manejarlo aquí
            }
            else
            {
                // Extrae el archivo
                using (FileStream fs = File.Create(entryPath))
                {
                    entry.Open().CopyTo(fs);
                }
            }
        }
    }
} 

Hhhhhh

using ICSharpCode.SharpZipLib.Core;
using ICSharpCode.SharpZipLib.Zip;Utiliza el siguiente código para descomprimir un archivo ZIP con contraseña:using System;
using System.IO;
using ICSharpCode.SharpZipLib.Core;
using ICSharpCode.SharpZipLib.Zip;

class Program
{
    static void Main(string[] args)
    {
        string zipFilePath = "archivo.zip"; // Ruta del archivo ZIP
        string extractPath = "carpeta_destino"; // Carpeta donde se extraerán los archivos
        string password = "tu_contraseña"; // Contraseña del archivo ZIP

        try
        {
            using (FileStream fs = new FileStream(zipFilePath, FileMode.Open, FileAccess.Read))
            {
                using (ZipInputStream zipStream = new ZipInputStream(fs))
                {
                    zipStream.Password = password;

                    ZipEntry entry;
                    while ((entry = zipStream.GetNextEntry()) != null)
                    {
                        if (!entry.IsFile)
                            continue;

                        string entryFileName = entry.Name;
                        string fullEntryPath = Path.Combine(extractPath, entryFileName);
                        string directory = Path.GetDirectoryName(fullEntryPath);

                        if (!Directory.Exists(directory))
                            Directory.CreateDirectory(directory);

                        using (FileStream outputStream = new FileStream(fullEntryPath, FileMode.Create, FileAccess.Write))
                        {
                            byte[] buffer = new byte[4096];
                            StreamUtils.Copy(zipStream, outputStream, buffer);
                        }
                    }
                }
            }

            Console.WriteLine("Descompresión exitosa.");
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error: " + ex.Message);
        }
    }
}

/////////

string zipFilePath = "ruta_del_archivo_zip.zip";
string extractFolder = "ruta_de_la_carpeta_de_extraccion";
string password = "tu_contraseña";

using (FileStream fs = File.OpenRead(zipFilePath))
{
    var loadOptions = new LoadOptions();
    loadOptions.Password = password;

    using (Archive archive = new Archive(fs, loadOptions))
    {
        foreach (var entry in archive.Entries)
        {
            string entryPath = Path.Combine(extractFolder, entry.Name);

            if (entry.IsDirectory)
            {
                // Crea directorios si no existen
                Directory.CreateDirectory(entryPath);
            }
            else
            {
                // Extrae archivos
                using (FileStream entryStream = File.Create(entryPath))
                {
                    entry.Extract(entryStream);
                }
            }
        }
    }
}

/////// 


using Aspose.Zip;
using System;

class Program
{
    static void Main()
    {
        // Ruta del archivo ZIP protegido por contraseña
        string archivoZip = "ruta/del/archivo.zip";
        
        // Contraseña que deseas validar
        string contraseña = "tu_contraseña";

        // Validar la contraseña antes de intentar la extracción
        if (EsContraseñaCorrecta(archivoZip, contraseña))
        {
            // La contraseña es correcta, ahora puedes extraer el archivo ZIP
            string directorioDestino = "ruta/de/destino";

            using (var archive = new ZipArchive(archivoZip, new ZipLoadOptions { Password = contraseña }))
            {
                foreach (var entry in archive.Entries)
                {
                    string destinoArchivo = Path.Combine(directorioDestino, entry.Name);
                    if (File.Exists(destinoArchivo))
                    {
                        Console.WriteLine($"El archivo {entry.Name} ya existe en el directorio de destino.");
                    }
                    else
                    {
                        entry.Extract(destinoArchivo);
                        Console.WriteLine($"El archivo {entry.Name} ha sido extraído.");
                    }
                }
            }
        }
        else
        {
            Console.WriteLine("La contraseña es incorrecta.");
        }
    }

    // Función para validar la contraseña de un archivo ZIP
    static bool EsContraseñaCorrecta(string archivoZip, string contraseña)
    {
        try
        {
            // Intentar abrir el archivo ZIP con la contraseña
            using (var archive = new ZipArchive(archivoZip, new ZipLoadOptions { Password = contraseña }))
            {
                // Si no se lanza una excepción, la contraseña es correcta
                return true;
            }
        }
        catch (InvalidPasswordException)
        {
            // La contraseña es incorrecta
            return false;
        }
    }
}