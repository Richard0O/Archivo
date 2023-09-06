-fgI78using System;
using System.Collections.Generic;
using System.IO;
using System.IO.Compression;

class Program
{
    static void Main()
    {
        string zipFilePath = "archivo.zip";
        string extractionPath = "extracted_files";
        string txtFilePath = "archivos.txt";
        string finalZipPath = "archivos_comprimidos.zip";

        ExtractZipFile(zipFilePath, extractionPath);
        List<string> fileList = GetFileList(extractionPath);
        ShowFileList(fileList);
        ConvertToTxt(fileList, txtFilePath);
        CompressToZip(txtFilePath, finalZipPath);

        Console.WriteLine("Proceso completado.");
    }

    static void ExtractZipFile(string zipFilePath, string extractionPath)
    {
        if (!Directory.Exists(extractionPath))
        {
            ZipFile.ExtractToDirectory(zipFilePath, extractionPath);
        }
    }

    static List<string> GetFileList(string folderPath)
    {
        List<string> fileList = new List<string>();
        fileList.AddRange(Directory.GetFiles(folderPath));
        return fileList;
    }

    static void ShowFileList(List<string> fileList)
    {
        Console.WriteLine("Archivos extraídos:");
        foreach (string file in fileList)
        {
            Console.WriteLine(file);
        }
    }

    static void ConvertToTxt(List<string> fileList, string txtFilePath)
    {
        using (StreamWriter writer = new StreamWriter(txtFilePath))
        {
            foreach (string file in fileList)
            {
                string content = File.ReadAllText(file);
                writer.WriteLine($"Contenido de {Path.GetFileName(file)}:");
                writer.WriteLine(content);
                writer.WriteLine();
            }
        }
    }

    static void CompressToZip(string txtFilePath, string finalZipPath)
    {
        string parentFolder = Path.GetDirectoryName(txtFilePath);
        string[] filesToZip = Directory.GetFiles(parentFolder, "*.txt");
        ZipFile.CreateFromDirectory(parentFolder, finalZipPath);
    }
}


//////////////////////////////////

using System;
using System.IO;
using System.IO.Compression;

class Program
{
    static void Main()
    {
        Console.WriteLine("Por favor, introduce la ruta del archivo ZIP:");
        string rutaArchivoZip = Console.ReadLine();

        Console.WriteLine("Por favor, introduce la carpeta de destino para la extracción:");
        string carpetaDestino = Console.ReadLine();

        try
        {
            // Verifica si el archivo ZIP existe
            if (File.Exists(rutaArchivoZip))
            {
                // Asegúrate de que la carpeta de destino exista o créala si no existe
                if (!Directory.Exists(carpetaDestino))
                {
                    Directory.CreateDirectory(carpetaDestino);
                }

                // Extrae el archivo ZIP en la carpeta de destino
                ZipFile.ExtractToDirectory(rutaArchivoZip, carpetaDestino);
                Console.WriteLine("Archivo ZIP extraído exitosamente.");
            }
            else
            {
                Console.WriteLine("El archivo ZIP especificado no existe.");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine("Ocurrió un error al extraer el archivo ZIP: " + ex.Message);
        }
    }
}


7777777
using ICSharpCode.SharpZipLib.Zip;
using System.IO;

public void ComprimirZip(string archivoOrigen, string archivoDestino)
{
    using (FileStream archivoDestinoStream = File.Create(archivoDestino))
    using (ZipOutputStream zipStream = new ZipOutputStream(archivoDestinoStream))
    {
        ZipEntry entry = new ZipEntry(Path.GetFileName(archivoOrigen));
        zipStream.PutNextEntry(entry);

        using (FileStream archivoOrigenStream = File.OpenRead(archivoOrigen))
        {
            byte[] buffer = new byte[4096];
            StreamUtils.Copy(archivoOrigenStream, zipStream, buffer);
        }
        zipStream.CloseEntry();
    }
}

\///////

comenzar.using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main()
    {
        string archivoRAR = "archivo.rar"; // Ruta del archivo RAR que deseas extraer
        string directorioDestino = "destino"; // Directorio donde deseas extraer los archivos

        try
        {
            using (Stream stream = File.OpenRead(archivoRAR))
            using (var reader = ReaderFactory.Open(stream))
            {
                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        // Validar si el archivo es seguro antes de extraerlo (opcional)
                        if (EsArchivoSeguro(reader.Entry))
                        {
                            reader.WriteEntryToDirectory(directorioDestino, new ExtractionOptions()
                            {
                                ExtractFullPath = true,
                                Overwrite = true // Si deseas sobrescribir archivos existentes
                            });
                            Console.WriteLine($"Extrayendo: {reader.Entry.Key}");
                        }
                        else
                        {
                            Console.WriteLine($"Archivo inseguro: {reader.Entry.Key}");
                        }
                    }
                }
            }

            Console.WriteLine("Extracción completada con éxito.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al extraer el archivo RAR: {ex.Message}");
        }
    }

    static bool EsArchivoSeguro(IEntry entry)
    {
        // Agrega tus propias validaciones de seguridad aquí si es necesario
        // Por ejemplo, puedes verificar la extensión del archivo o escanearlo con un antivirus.
        return true; // Cambia esto según tus necesidades
    }
}

\/////

using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main(string[] args)
    {
        string rarFilePath = "ruta_del_archivo.rar";
        string extractPath = "ruta_de_la_carpeta_destino";
        string password = "tu_contraseña";

        try
        {
            using (Stream stream = File.OpenRead(rarFilePath))
            using (var reader = ReaderFactory.Open(stream, new ReaderOptions() { Password = password }))
            {
                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        // Validar el tipo de archivo si es necesario
                        if (reader.Entry.Key.EndsWith(".txt", StringComparison.OrdinalIgnoreCase))
                        {
                            reader.WriteEntryToDirectory(extractPath, new ExtractionOptions()
                            {
                                ExtractFullPath = true,
                                Overwrite = true // Opcional: Sobrescribir si ya existe
                            });
                            Console.WriteLine($"Archivo extraído: {reader.Entry.Key}");
                        }
                        else
                        {
                            Console.WriteLine($"El archivo {reader.Entry.Key} no es un archivo válido.");
                        }
                    }
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al extraer archivos RAR: {ex.Message}");
        }
    }
}

########

using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Archives;
using SharpCompress.Readers;Utiliza el siguiente código para extraer un archivo RAR con contraseña:string archivoRAR = "ruta_del_archivo.rar";
string destino = "ruta_de_destino";
string contraseña = "contraseña_del_archivo";

using (Stream stream = File.OpenRead(archivoRAR))
{
    var readerOptions = new ReaderOptions
    {
        Password = contraseña // Establece la contraseña aquí
    };

    var archive = ArchiveFactory.Open(stream, readerOptions);
    
    foreach (var entry in archive.Entries)
    {
        if (!entry.IsDirectory)
        {
            entry.WriteToDirectory(destino, new ExtractionOptions
            {
                ExtractFullPath = true,
                Overwrite = true
            });
        }
    }
}

@aaaaaaa

using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main(string[] args)
    {
        string rutaArchivoRAR = "archivo.rar";
        string directorioDestino = "directorio_destino";
        string contraseña = "tu_contraseña";

        // Validar la contraseña antes de la extracción (opcional)
        bool contraseñaValida = ValidarContraseña(rutaArchivoRAR, contraseña);

        if (contraseñaValida)
        {
            // Extraer el archivo RAR
            using (Stream stream = File.OpenRead(rutaArchivoRAR))
            using (var reader = ReaderFactory.Open(stream, new ReaderOptions { Password = contraseña }))
            {
                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        reader.WriteEntryToDirectory(directorioDestino, ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
                    }
                }
            }
        }
        else
        {
            Console.WriteLine("Contraseña incorrecta.");
        }
    }

    static bool ValidarContraseña(string rutaArchivoRAR, string contraseña)
    {
        using (Stream stream = File.OpenRead(rutaArchivoRAR))
        using (var reader = ReaderFactory.Open(stream, new ReaderOptions { Password = contraseña }))
        {
            while (reader.MoveToNextEntry())
            {
                if (reader.Entry.IsEncrypted)
                {
                    // Si la entrada está cifrada, la contraseña es válida
                    return true;
                }
            }
        }
        // Si no hay entradas cifradas, la contraseña es incorrecta
        return false;
    }
}Asegúrate de reemplazar "archivo.rar", "