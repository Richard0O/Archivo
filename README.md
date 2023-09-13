Descomprimir aquí:using (var stream = File.OpenRead("archivo.rar"))
{
    using (var reader = RarReader.Open(stream))
    {
        while (reader.MoveToNextEntry())
        {
            if (!reader.Entry.IsDirectory)
            {
                reader.WriteEntryToDirectory(".", new ExtractionOptions()
                {
                    ExtractFullPath = true,
                    Overwrite = true
                });
            }
        }
    }
}Descomprimir creando una carpeta con el mismo nombre del archivo RAR:using (var stream = File.OpenRead("archivo.rar"))
{
    using (var reader = RarReader.Open(stream))
    {
        while (reader.MoveToNextEntry())
        {
            if (!reader.Entry.IsDirectory)
            {
                string folderName = Path.GetFileNameWithoutExtension("archivo.rar");
                string destinationFolder = Path.Combine(".", folderName);

                reader.WriteEntryToDirectory(destinationFolder, new ExtractionOptions()
                {
                    ExtractFullPath = true,
                    Overwrite = true
                });
            }
        }
    }
}Descomprimir varios archivos: Simplemente repite el código anterior para cada archivo RAR que quieras descomprimir.Descomprimir archivos RAR con contraseña: Puedes establecer la contraseña en las opciones de extracción de la siguiente manera:ExtractionOptions options = new ExtractionOptions()
{
    ExtractFullPath = true,
    Overwrite = true,
    Password = "tu_contraseña"
};

reader.WriteEntryToDirectory(".", options);


Tttttgggg

using System;
using System.IO;
using System.Linq;
using SharpCompress.Archives;
using SharpCompress.Archives.Rar;
using SharpCompress.Common;

class Program
{
    static void Main()
    {
        string archivoRAR = "ruta_del_archivo.rar";
        string destino = "carpeta_de_destino";

        using (Stream stream = File.OpenRead(archivoRAR))
        using (var archive = RarArchive.Open(stream))
        {
            foreach (var entry in archive.Entries)
            {
                if (!entry.IsDirectory)
                {
                    string destinationPath = Path.Combine(destino, entry.Key);

                    if (File.Exists(destinationPath))
                    {
                        Console.Write($"El archivo {entry.Key} ya existe. ¿Desea reemplazarlo? (S/N): ");
                        var respuesta = Console.ReadLine();

                        if (respuesta.Trim().ToUpper() != "S")
                        {
                            continue; // No reemplazar el archivo
                        }
                    }

                    entry.WriteTo(destinationPath, new ExtractionOptions()
                    {
                        ExtractFullPath = true,
                        Overwrite = true
                    });

                    Console.WriteLine($"Archivo {entry.Key} extraído.");
                }
            }
        }
    }
}