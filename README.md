I8Descomprimir aquí:using (var stream = File.OpenRead("archivo.rar"))
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


Yyyyyyyyyyyyy

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main(string[] args)
    {
        CompressFile("archivo.txt", "archivo.rar");
        CompressFolder("carpetas", "carpetas.rar");
        CompressWithPassword("documento.txt", "documento.rar", "miContraseña");
    }

    // Método para comprimir un archivo
    static void CompressFile(string sourceFilePath, string destinationArchivePath)
    {
        using (var archive = ArchiveFactory.Create(ArchiveType.Rar))
        {
            archive.AddEntry(Path.GetFileName(sourceFilePath), sourceFilePath);
            archive.SaveTo(destinationArchivePath, CompressionType.Rar);
        }
    }

    // Método para comprimir varias carpetas en una carpeta
    static void CompressFolder(string sourceFolderPath, string destinationArchivePath)
    {
        using (var archive = ArchiveFactory.Create(ArchiveType.Rar))
        {
            archive.AddAllFromDirectory(sourceFolderPath);
            archive.SaveTo(destinationArchivePath, CompressionType.Rar);
        }
    }

    // Método para comprimir un archivo con contraseña
    static void CompressWithPassword(string sourceFilePath, string destinationArchivePath, string password)
    {
        using (var archive = ArchiveFactory.Create(ArchiveType.Rar))
        {
            archive.AddEntry(Path.GetFileName(sourceFilePath), sourceFilePath);
            archive.SaveTo(destinationArchivePath, new CompressionInfo { Type = CompressionType.Rar, Password = password });
        }
    }
}

Iiiiikki

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;Define una clase que contenga los métodos para las operaciones de compresión:class CompressionManager
{
    public static void CompressFile(string sourceFile, string destinationArchive)
    {
        using (var archive = ArchiveFactory.Create(ArchiveType.Zip))
        {
            archive.AddEntry(Path.GetFileName(sourceFile), sourceFile);
            archive.SaveTo(destinationArchive, CompressionType.Deflate);
        }
    }

    public static void CompressFolder(string sourceFolder, string destinationArchive)
    {
        using (var archive = ArchiveFactory.Create(ArchiveType.Zip))
        {
            archive.AddAllFromDirectory(sourceFolder);
            archive.SaveTo(destinationArchive, CompressionType.Deflate);
        }
    }

    public static void CompressToFolder(string sourceFolder, string destinationFolder)
    {
        Directory.CreateDirectory(destinationFolder);

        string archiveFileName = Path.Combine(destinationFolder, "compressed.zip");

        using (var archive = ArchiveFactory.Create(ArchiveType.Zip))
        {
            archive.AddAllFromDirectory(sourceFolder);
            archive.SaveTo(archiveFileName, CompressionType.Deflate);
        }
    }

    public static void CompressWithPassword(string sourceFile, string destinationArchive, string password)
    {
        using (var archive = ArchiveFactory.Create(ArchiveType.Zip))
        {
            archive.AddEntry(Path.GetFileName(sourceFile), sourceFile);
            archive.SaveTo(destinationArchive, CompressionType.Deflate, new ZipWriterOptions(CompressionType.Deflate)
            {
                Password = password
            });
        }
    }
}Luego, puedes llamar a estos métodos desde tu programa principal:class Program
{
    static void Main(string[] args)
    {
        string sourceFile = "archivo.txt";
        string sourceFolder = "carpeta";
        string destinationArchive = "archivo.zip";
        string destinationFolder = "carpeta_comprimida";
        string password = "miContraseña";

        CompressionManager.CompressFile(sourceFile, destinationArchive);
        CompressionManager.CompressFolder(sourceFolder, destinationArchive);
        CompressionManager.CompressToFolder(sourceFolder, destinationFolder);
        CompressionManager.CompressWithPassword(sourceFile, destinationArchive, password);
    }
}


using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main(string[] args)
    {
        string archivoComprimido = "archivo.rar"; // Cambia esto al nombre de tu archivo comprimido
        string directorioDestino = "destino"; // Cambia esto al directorio donde deseas descomprimir

        try
        {
            using (var stream = File.OpenRead(archivoComprimido))
            {
                var reader = ReaderFactory.Open(stream);

                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        string destinationFileName = Path.Combine(directorioDestino, reader.Entry.Key);

                        if (File.Exists(destinationFileName))
                        {
                            Console.WriteLine($"El archivo {reader.Entry.Key} ya existe. ¿Deseas reemplazarlo? (S/N)");
                            string respuesta = Console.ReadLine();
                            if (respuesta.Trim().ToUpper() != "S")
                            {
                                continue; // Salta este archivo si no deseas reemplazarlo
                            }
                        }

                        reader.WriteEntryToDirectory(directorioDestino, new ExtractionOptions()
                        {
                            ExtractFullPath = true,
                            Overwrite = true // Permite reemplazar archivos existentes
                        });
                        Console.WriteLine($"Se descomprimió {reader.Entry.Key} a {destinationFileName}");
                    }
                }
            }

            Console.WriteLine("Descompresión completa.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al descomprimir: {ex.Message}");
        }
    }
}