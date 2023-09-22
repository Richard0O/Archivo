


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