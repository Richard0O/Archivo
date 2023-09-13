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



Mmmmmmm

using System.IO;
using System.Linq;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Writers;

string archivoOrigen = "archivo.txt";
string archivoDestino = "archivo.rar";

using (var stream = new FileStream(archivoDestino, FileMode.Create))
{
    using (var writer = WriterFactory.Open(stream, ArchiveType.Rar))
    {
        writer.Write(archivoOrigen, Path.GetFileName(archivoOrigen));
    }
}Comprimir varios archivos en un archivo RAR:using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Writers;

string[] archivosOrigen = new string[] { "archivo1.txt", "archivo2.txt" };
string archivoDestino = "archivos.rar";

using (var stream = new FileStream(archivoDestino, FileMode.Create))
{
    using (var writer = WriterFactory.Open(stream, ArchiveType.Rar))
    {
        foreach (var archivo in archivosOrigen)
        {
            writer.Write(archivo, Path.GetFileName(archivo));
        }
    }
}Comprimir archivos RAR con contraseña:using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Writers;

string archivoOrigen = "archivo.txt";
string archivoDestino = "archivo_con_contraseña.rar";
string contraseña = "tu_contraseña";

var opciones = new CompressionInfo()
{
    Password = contraseña
};

using (var stream = new FileStream(archivoDestino, FileMode.Create))
{
    using (var writer = WriterFactory.Open(stream, ArchiveType.Rar, opciones))
    {
        writer.Write(archivoOrigen, Path.GetFileName(archivoOrigen));
    }
}


Yyyyyyyy

Comprimir un archivo:using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Archives;

string archivoOrigen = "archivo.txt";
string archivoDestino = "archivo.rar";

using (Stream stream = File.OpenWrite(archivoDestino))
{
    using (var archive = ArchiveFactory.Create(ArchiveType.Rar, stream))
    {
        archive.AddEntry("archivo.txt", archivoOrigen);
    }
}Comprimir varias carpetas:using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Archives;

string carpetaOrigen = "carpeta";
string archivoDestino = "carpeta.rar";

using (Stream stream = File.OpenWrite(archivoDestino))
{
    using (var archive = ArchiveFactory.Create(ArchiveType.Rar, stream))
    {
        archive.AddAllFromDirectory(carpetaOrigen, new ExtractionOptions()
        {
            ExtractFullPath = true
        });
    }
}Comprimir creando una carpeta:using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Archives;

string carpetaOrigen = "carpeta";
string archivoDestino = "carpeta.rar";

using (Stream stream = File.OpenWrite(archivoDestino))
{
    using (var archive = ArchiveFactory.Create(ArchiveType.Rar, stream))
    {
        archive.AddAllFromDirectory(carpetaOrigen, new ExtractionOptions()
        {
            ExtractFullPath = true,
            Overwrite = true,
            WriteType = WriteType.Default,
            ArchiveEncoding = new ArchiveEncoding()
            {
                ArchiveEncodingType = ArchiveEncodingType.Default
            }
        });
    }
}Comprimir varios archivos:using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Archives;

string[] archivosOrigen = { "archivo1.txt", "archivo2.txt" };
string archivoDestino = "archivos.rar";

using (Stream stream = File.OpenWrite(archivoDestino))
{
    using (var archive = ArchiveFactory.Create(ArchiveType.Rar, stream))
    {
        foreach (var archivo in archivosOrigen)
        {
            archive.AddEntry(Path.GetFileName(archivo), archivo);
        }
    }
}Comprimir archivos RAR con contraseña:using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Archives;
using SharpCompress.Archives.Rar;

string archivoOrigen = "archivo.rar";
string archivoDestino = "archivo_con_password.rar";
string contraseña = "tu_contraseña";

using (Stream stream = File.OpenWrite(archivoDestino))
{
    using (var archive = RarArchive.Create())
    {
        archive.AddEntry("archivo.txt", archivoOrigen, new WriterOptions(CompressionType.Rar))
        {
            Password = contraseña
        };

        archive.SaveTo(stream, new ArchiveWriterOptions(CompressionType.Rar));
    }
}