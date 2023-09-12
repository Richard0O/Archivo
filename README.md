Archivos 
Ttttttt


using System;
using System.IO;
using System.Linq;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main(string[] args)
    {
        string sourcePath = "ruta_del_archivo.rar"; // Ruta del archivo RAR
        string destinationFolder = "carpeta_destino"; // Carpeta de destino

        try
        {
            // Descomprimir el archivo RAR en la carpeta de destino
            ExtractRAR(sourcePath, destinationFolder);

            // Descomprimir varios archivos RAR en una carpeta
            string multipleSourcePath = "carpeta_con_varios_archivos_rar";
            string multipleDestinationFolder = "carpeta_destino_varios";
            ExtractMultipleRARs(multipleSourcePath, multipleDestinationFolder);

            // Descomprimir archivo RAR con contraseña
            string passwordProtectedRAR = "archivo_con_contraseña.rar";
            string password = "tu_contraseña";
            string passwordProtectedDestination = "carpeta_destino_contraseña";
            ExtractRARWithPassword(passwordProtectedRAR, password, passwordProtectedDestination);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }

    // Método para descomprimir un archivo RAR en una carpeta específica
    static void ExtractRAR(string sourcePath, string destinationFolder)
    {
        using (var stream = File.OpenRead(sourcePath))
        using (var reader = ReaderFactory.Open(stream))
        {
            while (reader.MoveToNextEntry())
            {
                if (!reader.Entry.IsDirectory)
                {
                    reader.WriteEntryToDirectory(destinationFolder, ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
                }
            }
        }
    }

    // Método para descomprimir varios archivos RAR en una carpeta
    static void ExtractMultipleRARs(string sourceFolderPath, string destinationFolder)
    {
        foreach (var sourcePath in Directory.GetFiles(sourceFolderPath, "*.rar"))
        {
            ExtractRAR(sourcePath, destinationFolder);
        }
    }

    // Método para descomprimir un archivo RAR protegido con contraseña
    static void ExtractRARWithPassword(string sourcePath, string password, string destinationFolder)
    {
        var options = new ExtractionOptions
        {
            Password = password
        };

        using (var stream = File.OpenRead(sourcePath))
        using (var reader = ReaderFactory.Open(stream, options))
        {
            while (reader.MoveToNextEntry())
            {
                if (!reader.Entry.IsDirectory)
                {
                    reader.WriteEntryToDirectory(destinationFolder, ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
                }
            }
        }
    }
}

////////////////////////////////////////////7


using System;
using System.Collections.Generic;
using System.IO;
using System.IO.Compression;

class Program
{
    static void Main(string[] args)
    {
        try
        {
            // 1. Extraer un archivo ZIP
            string zipPath = "archivozip.zip";
            string extractPath = @"C:\Users\ricky\Desktop\Archivo2\extraccion";
            string newZipPath = "ArchivoComprimido.zip";
            string manifestPath = "Archivo Manifiesto.txt";

            ExtractZipFile(zipPath, extractPath);

            //Leer archivos y mostrarlos en una lista
            List<string> fileList = ListFiles(extractPath);
            Console.WriteLine("Archivos extraídos:");
            foreach (var file in fileList)
            {
                Console.WriteLine(file);
            }

            //Crear un archivo manifiesto
            CreateManifest(fileList, manifestPath);
            Console.WriteLine("Archivo manifiesto creado: " + manifestPath);

            //Comprimir archivos en un nuevo archivo ZIP

            CompressFiles(fileList, newZipPath);
            Console.WriteLine("Archivos comprimidos en: " + newZipPath);
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error: " + ex.Message);
        }
    }

    // Método para extraer un archivo ZIP
    static void ExtractZipFile(string zipPath, string extractPath)
    {
        try
        {

            ZipFile.ExtractToDirectory(zipPath, extractPath);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al comprimir en archivo ZIP: {ex.Message}");

            if (!File.Exists(zipPath))
            {
                throw new FileNotFoundException("El archivo ZIP no existe.");
            }

            if (!Directory.Exists(extractPath))
            {
                Directory.CreateDirectory(extractPath);
            }
        }
    }



    // Método para listar archivos en una carpeta y subdirectorios
    static List<string> ListFiles(string folderPath)
    {

        List<string> fileList = new List<string>();
        foreach (var file in fileList)
        {
            Console.WriteLine(file);
        }
        foreach (string file in Directory.GetFiles(folderPath, "*", SearchOption.AllDirectories))
        {

            fileList.Add(file);

        }
        return fileList;
    }

    // Método para crear un archivo manifiesto
    static void CreateManifest(List<string> fileList, string manifestPath)
    {
        try
        {

            File.WriteAllLines(manifestPath, fileList);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al crear Manifiesto: {ex.Message}");
        }
    }

    // Método para comprimir archivos en un nuevo archivo ZIP
    static void CompressFiles(List<string> fileList, string newZipPath)
    {
        try
        {

            using (ZipArchive archive = ZipFile.Open(newZipPath, ZipArchiveMode.Create))
            {
                foreach (string file in fileList)
                {
                    archive.CreateEntryFromFile(file, Path.GetFileName(file));
                }

            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al Lista los archivos: {ex.Message}");
        }
    }
}

/////////////////////////////////////////777



using System;
using System.Collections.Generic;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Writers;
using System.Linq;
using SharpCompress.Readers;

class Program
{
    static void Main()
    {
        string zipPath = "archivozip.zip";
        string extractPath = @"C:\Users\ricky\Desktop\ArchivoZip\Extraccion";
        string newZipPath = "ArchivoComprimido.zip";
        string manifestPath = "Archivo Manifiesto.txt";

        try
        {
            //Extraer el archivo 
             ExtractZipFile(zipPath, extractPath);

            //Leer y mostrar los archivos extraídos
            List<string> archivosExtraidos = ListarArchivos(extractPath);
            Console.WriteLine("Archivos extraídos:");
            foreach (string archivo in archivosExtraidos)
            {
                Console.WriteLine(archivo);                                                                                     
            }

            // Paso 3: Crear el archivo manifiesto
            CrearManifiesto(extractPath, manifestPath);

            // Paso 4: Comprimir en un archivo ZIP
            ComprimirEnZip(extractPath, newZipPath);
            Console.WriteLine("Se ha creado el archivo ZIP.");

            // Limpieza: Eliminar la carpeta de extracción
            //Directory.Delete(carpetaExtraccion, true);


        }
        catch (Exception ex)
        {
            Console.WriteLine("Error: " + ex.Message);
        }
    }
   // Método para extraer un archivo ZIP
    static void ExtractZipFile(string zipPath, string extractPath)
    {
        try
        {
            using (var stream = File.OpenRead(zipPath))
            {
                var reader = ReaderFactory.Open(stream);
                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        reader.WriteEntryToDirectory(extractPath, new ExtractionOptions()
                        {
                            ExtractFullPath = true,
                            Overwrite = true
                        });
                    }
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error al extraer el Archivo: " + ex.Message);
        }
    }
// Método para listar archivos en una carpeta y subdirectorios
    static List<string> ListarArchivos(string extractPath)
    {
        return Directory.GetFiles(extractPath, "*", SearchOption.AllDirectories).ToList();
    }

    // Método para crear un archivo manifiesto
    static void CrearManifiesto(string extractPath, string manifestPath)
    {
        try
        {
            var archivos = ListarArchivos(extractPath);
            File.WriteAllLines(manifestPath, archivos);
            Console.WriteLine("Se ha creado el archivo manifiesto.");

        }
        catch (Exception ex)
        {
            Console.WriteLine("Error al crear el Archivo Manifiesto: " + ex.Message);
        }

    }

    // Método para comprimir archivos en un nuevo archivo ZIP
    static void ComprimirEnZip(string extractionPath, string newZipPath)
    {
        try
        {
            using (var stream = new FileStream(newZipPath, FileMode.Create))
            using (var writer = WriterFactory.Open(stream, ArchiveType.Zip, new WriterOptions(CompressionType.Deflate)))
            {
                {
                    writer.WriteAll(extractionPath, "*", SearchOption.AllDirectories);
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error al crear el Archivo Manifiesto: " + ex.Message);
        }

    }
}
Bbbbbbbbb


using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main()
    {
        string archivoRAR = "ruta_del_archivo.rar";
        string destino = "carpeta_destino";
        string contraseña = "tu_contraseña";

        using (Stream stream = File.OpenRead(archivoRAR))
        using (var reader = ReaderFactory.Open(stream, new ReaderOptions { Password = contraseña }))
        {
            while (reader.MoveToNextEntry())
            {
                if (!reader.Entry.IsDirectory)
                {
                    reader.WriteEntryToDirectory(destino, new ExtractionOptions
                    {
                        ExtractFullPath = true,
                        Overwrite = true
                    });
                }
            }
        }

        Console.WriteLine("¡Archivo RAR descomprimido con éxito!");
    }



111222111
 

```csharp
using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Ingrese la ruta del archivo RAR:");
        string archivoRar = Console.ReadLine();

        Console.WriteLine("Ingrese la contraseña del archivo RAR:");
        string contraseña = Console.ReadLine();

        try
        {
            using (var stream = File.OpenRead(archivoRar))
            {
                var reader = ReaderFactory.Open(stream, new ReaderOptions
                {
                    Password = contraseña // Establece la contraseña proporcionada por el usuario
                });

                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        reader.WriteEntryToDirectory("ruta_de_destino", ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
                    }
                }
            }

            Console.WriteLine("Descompresión completada.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
}
```nnnnn


using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Archives;
using SharpCompress.Writers;

class Program
{
    static void Main(string[] args)
    {
        string sourceDirectory = @"C:\Ruta\De\Origen";
        string rarFilePath = @"C:\Ruta\Del\Archivo\Rar\rarchivo.rar";

        using (var archive = ArchiveFactory.Create(ArchiveType.Rar, rarFilePath, CompressionType.Rar))
        {
            foreach (var filePath in Directory.EnumerateFiles(sourceDirectory))
            {
                archive.AddEntry(Path.GetFileName(filePath), filePath);
            }
        }

        Console.WriteLine("Archivos comprimidos con éxito.");
    }
}

/mmmmmm

using (var archive = ArchiveFactory.Create(ArchiveType.Rar))
{
    archive.AddEntry("nombre_del_archivo.rar", "ruta_del_archivo_a_comprimir.txt");
    archive.SaveTo("ruta_de_la_carpeta_destino");
}Comprimir varios archivos en una carpeta en un archivo RAR:using (var archive = ArchiveFactory.Create(ArchiveType.Rar))
{
    var archivos = new List<string>
    {
        "ruta_del_archivo1.txt",
        "ruta_del_archivo2.txt"
        // Agrega más archivos según sea necesario
    };

    foreach (var archivo in archivos)
    {
        archive.AddEntry(Path.GetFileName(archivo), archivo);
    }

    archive.SaveTo("ruta_de_la_carpeta_destino");
}Agregar contraseña a un archivo RAR:using (var archive = ArchiveFactory.Create(ArchiveType.Rar))
{
    archive.AddEntry("nombre_del_archivo.rar", "ruta_del_archivo_a_comprimir.txt");
    archive.Password = "tu_contraseña";
    archive.SaveTo("ruta_de_la_carpeta_destino");
}