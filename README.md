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

Aaaaaaaa

using System;
using System.IO;
using System.Linq;
using SharpCompress.Common;
using SharpCompress.Readers;
using SharpCompress.Writers;

class Program
{
    static void Main()
    {
        string archivoRAR = "archivo.rar";
        string directorioDestino = "directorio_destino";

        if (File.Exists(archivoRAR))
        {
            using (Stream stream = File.OpenRead(archivoRAR))
            using (var reader = ReaderFactory.Open(stream))
            {
                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        string destino = Path.Combine(directorioDestino, reader.Entry.Key);
                        if (File.Exists(destino))
                        {
                            Console.WriteLine($"El archivo {reader.Entry.Key} ya existe. Sobrescribiendo.");
                            File.Delete(destino);
                        }
                        else
                        {
                            Console.WriteLine($"Extrayendo {reader.Entry.Key}");
                        }

                        reader.WriteEntryToDirectory(directorioDestino, new ExtractionOptions()
                        {
                            ExtractFullPath = true,
                            Overwrite = true
                        });
                    }
                }
            }

            Console.WriteLine("Extracción completada.");
        }
        else
        {
            Console.WriteLine($"El archivo {archivoRAR} no existe.");
        }
    }
}

6666666

using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main(string[] args)
    {
        string archivoRar = "ruta_del_archivo.rar";
        string directorioDestino = "ruta_del_directorio_destino";

        try
        {
            using (Stream stream = File.OpenRead(archivoRar))
            using (var reader = ReaderFactory.Open(stream))
            {
                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        string outputPath = Path.Combine(directorioDestino, reader.Entry.Key);

                        if (File.Exists(outputPath))
                        {
                            Console.WriteLine($"El archivo {reader.Entry.Key} ya existe. ¿Desea reemplazarlo? (S/N)");
                            string respuesta = Console.ReadLine();
                            if (respuesta.Equals("N", StringComparison.OrdinalIgnoreCase))
                            {
                                continue;
                            }
                        }

                        reader.WriteEntryToDirectory(directorioDestino, new ExtractionOptions()
                        {
                            ExtractFullPath = true,
                            Overwrite = true // Esto permite reemplazar archivos existentes
                        });
                    }
                }
            }

            Console.WriteLine("Descompresión completada con éxito.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al descomprimir el archivo: {ex.Message}");
        }
    }
}