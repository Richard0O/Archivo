using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main(string[] args)
    {
        string archivoRar = "ruta\\al\\archivo.rar"; // Reemplaza con la ruta de tu archivo RAR

        using (Stream stream = File.OpenRead(archivoRar))
        {
            var reader = ArchiveFactory.Open(stream);
            foreach (var entry in reader.Entries)
            {
                if (!entry.IsDirectory)
                {
                    entry.WriteToDirectory(Path.GetDirectoryName(archivoRar), new ExtractionOptions()
                    {
                        ExtractFullPath = true,
                        Overwrite = true
                    });
                }
            }
        }

        Console.WriteLine("Descompresión completada.");
    }
}

////////

string archivoRar = "ruta/del/archivo.rar";
string carpetaDestino = Path.GetDirectoryName(archivoRar); // Obtiene la carpeta donde se encuentra el archivo RAR

using (var archivo = RarArchive.Open(archivoRar))
{
    foreach (var entrada in archivo.Entries)
    {
        if (!entrada.IsDirectory)
        {
            entrada.WriteToDirectory(carpetaDestino, new ExtractionOptions()
            {
                ExtractFullPath = true,
                Overwrite = true
            });
        }
    }
}

Console.WriteLine("Archivo RAR descomprimido con éxito.");