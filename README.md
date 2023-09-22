


string archivoRar = "ArchivoRar.rar"; // Reemplaza esto con la ruta de tu archivo RAR
        string carpetaDestino = "Extraccion"; // Reemplaza esto con la ruta de la carpeta de destino

        // Obtén el nombre del archivo RAR sin extensión
        string nombreSinExtension = Path.GetFileNameWithoutExtension(archivoRar);

        // Crea la carpeta de destino con el nombre del archivo RAR
        string carpetaDestinoRenombrada = Path.Combine(carpetaDestino, nombreSinExtension);

        // Asegúrate de que la carpeta de destino exista o créala si no existe
        if (!Directory.Exists(carpetaDestinoRenombrada))
        {
            Directory.CreateDirectory(carpetaDestinoRenombrada);
        }

        // Extrae el archivo RAR en la carpeta de destino renombrada
        using (var archive = ArchiveFactory.Open(archivoRar))
        {
            foreach (var entry in archive.Entries)
            {
                if (!entry.IsDirectory)
                {
                    entry.WriteToDirectory(carpetaDestinoRenombrada, new ExtractionOptions()
                    {
                        ExtractFullPath = true,
                        Overwrite = true
                    });
                }
            }
        }

        Console.WriteLine("Archivo RAR extraído y carpeta de destino renombrada con éxito.");



Hhhhh

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Writers;

class Program
{
    static void Main()
    {
        string archivoAComprimir = "archivo.txt"; // Cambia esto al nombre de tu archivo
        string archivoComprimido = "archivo.rar"; // Nombre del archivo comprimido
        string contraseña = "mi_contraseña"; // Cambia esto a tu contraseña

        using (Stream stream = new FileStream(archivoComprimido, FileMode.Create))
        {
            using (var writer = WriterFactory.Open(stream, ArchiveType.Rar, new WriterOptions(CompressionType.Rar, new RarWriterOptions { Password = contraseña })))
            {
                writer.Write(archivoAComprimir, Path.GetFileName(archivoAComprimir));
            }
        }

        Console.WriteLine("Archivo comprimido y protegido con contraseña.");
    }
}


] yyyyyyyyy

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Writers;

class Program
{
    static void Main()
    {
        string archivoRar = "archivo.rar";
        string directorioArchivos = "archivos_a_comprimir";
        string contraseña = "tu_contraseña";

        using (var archive = ArchiveFactory.Open(archivoRar, Options.KeepStreamsOpen))
        {
            foreach (var archivo in Directory.EnumerateFiles(directorioArchivos))
            {
                var entry = archive.AddEntry(Path.GetFileName(archivo));

                // Establecer la contraseña
                entry.Password = contraseña;

                using (var streamWriter = new StreamWriter(entry.OpenEntryStream()))
                using (var fileStream = File.OpenRead(archivo))
                {
                    fileStream.CopyTo(streamWriter.BaseStream);
                }
            }
        }

        Console.WriteLine("Archivos comprimidos con éxito.");
    }
}

Hhhhhh

using System;
using System.Collections.Generic;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main()
    {
        // Ruta del archivo RAR de salida
        string outputPath = "archivo_comprimido.rar";

        // Crear una lista de archivos que deseas comprimir
        List<string> filesToCompress = new List<string>
        {
            "archivo1.txt",
            "archivo2.txt",
            // Agrega más archivos si es necesario
        };

        // Comprimir los archivos en un archivo RAR
        using (Stream stream = File.OpenWrite(outputPath))
        {
            using (var archive = ArchiveFactory.Create(ArchiveType.Rar, stream))
            {
                foreach (var file in filesToCompress)
                {
                    archive.AddEntry(file, File.OpenRead(file), true);
                }
            }
        }

        Console.WriteLine("Archivos comprimidos con éxito.");
    }
}


44444

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main()
    {
        // Ruta de la carpeta que deseas comprimir
        string carpetaAComprimir = @"C:\Ruta\De\Tu\Carpeta";

        // Nombre y ubicación del archivo RAR resultante
        string archivoRar = @"C:\Ruta\De\Tu\Archivo.rar";

        // Crear un archivo RAR
        using (var archive = ArchiveFactory.Create(ArchiveType.Rar, archivoRar))
        {
            // Agregar todos los archivos y carpetas de la carpeta a comprimir
            foreach (var archivo in Directory.EnumerateFiles(carpetaAComprimir, "*", SearchOption.AllDirectories))
            {
                var archivoEntry = archive.CreateEntry(Path.GetRelativePath(carpetaAComprimir, archivo), CompressionType.Default);

                using (var streamWriter = archivoEntry.OpenEntryStreamWriter())
                using (var archivoStream = File.OpenRead(archivo))
                {
                    archivoStream.CopyTo(streamWriter.BaseStream);
                }
            }
        }

        Console.WriteLine("Carpeta comprimida con éxito en un archivo RAR.");
    }
}