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


Tttttt

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main()
    {
        string archivoComprimido = "ruta/archivo.rar"; // Ruta de tu archivo comprimido
        string carpetaDestino = "ruta/destino"; // Carpeta donde se extraerán los archivos
        
        try
        {
            using (Stream stream = File.OpenRead(archivoComprimido))
            {
                var reader = ReaderFactory.Open(stream);

                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        string outputPath = Path.Combine(carpetaDestino, reader.Entry.Key);

                        if (File.Exists(outputPath))
                        {
                            Console.WriteLine($"El archivo {reader.Entry.Key} ya existe. ¿Deseas reemplazarlo? (S/N)");

                            if (Console.ReadLine()?.Trim().ToUpper() == "S")
                            {
                                reader.WriteEntryTo(outputPath, ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
                            }
                        }
                        else
                        {
                            reader.WriteEntryTo(outputPath, ExtractOptions.ExtractFullPath);
                        }
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


555555


using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main()
    {
        try
        {
            string archivoComprimido = "archivo.zip"; // Reemplaza con la ruta de tu archivo comprimido
            string directorioDestino = "destino"; // Reemplaza con la carpeta donde deseas extraer los archivos

            using (Stream stream = File.OpenRead(archivoComprimido))
            using (var reader = ReaderFactory.Open(stream))
            {
                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        var entry = reader.Entry;

                        string destinoArchivo = Path.Combine(directorioDestino, entry.Key);
                        
                        // Verificar si el archivo ya existe en el destino
                        if (File.Exists(destinoArchivo))
                        {
                            Console.WriteLine($"El archivo {entry.Key} ya existe en el destino.");
                            Console.Write("¿Deseas reemplazarlo? (Sí/No): ");
                            string respuesta = Console.ReadLine();

                            if (respuesta.Equals("No", StringComparison.OrdinalIgnoreCase))
                            {
                                continue; // Saltar este archivo
                            }
                        }

                        // Extraer el archivo
                        entry.WriteToDirectory(directorioDestino, new ExtractionOptions
                        {
                            ExtractFullPath = true,
                            Overwrite = true // Esto reemplazará el archivo si ya existe
                        });
                    }
                }
            }

            Console.WriteLine("Descompresión completada.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al descomprimir: {ex.Message}");
        }
    }
}
```

Este código verifica

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main()
    {
        try
        {
            string archivoRar = "archivo.rar";
            string directorioDestino = "directorioDestino";

            using (var archivo = ArchiveFactory.Open(archivoRar))
            {
                foreach (var entrada in archivo.Entries)
                {
                    if (!entrada.IsDirectory)
                    {
                        // Si deseas reemplazar archivos existentes, descomenta la siguiente línea
                        // entrada.WriteToDirectory(directorioDestino, new ExtractionOptions() { ExtractFullPath = true, Overwrite = true });

                        // Si deseas preguntar antes de reemplazar, puedes usar el siguiente código
                        string rutaArchivoDestino = Path.Combine(directorioDestino, entrada.Key);
                        if (File.Exists(rutaArchivoDestino))
                        {
                            Console.WriteLine($"El archivo {entrada.Key} ya existe. ¿Deseas reemplazarlo? (S/N)");
                            string respuesta = Console.ReadLine();
                            if (respuesta.Trim().ToUpper() == "S")
                            {
                                entrada.WriteToDirectory(directorioDestino, new ExtractionOptions() { ExtractFullPath = true, Overwrite = true });
                            }
                        }
                        else
                        {
                            entrada.WriteToDirectory(directorioDestino, new ExtractionOptions() { ExtractFullPath = true, Overwrite = false });
                        }
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

Iiiiiii

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main()
    {
        string rutaArchivoRAR = "ruta/al/archivo.rar";
        string carpetaDestino = "ruta/de/destino/";

        try
        {
            using (var archivo = ArchiveFactory.Open(rutaArchivoRAR))
            {
                foreach (var entrada in archivo.Entries)
                {
                    if (!entrada.IsDirectory)
                    {
                        string rutaDestino = Path.Combine(carpetaDestino, entrada.Key);

                        // Verificar si el archivo ya existe y preguntar si deseas reemplazarlo
                        if (File.Exists(rutaDestino))
                        {
                            Console.WriteLine($"El archivo {entrada.Key} ya existe. ¿Deseas reemplazarlo? (S/N)");
                            var respuesta = Console.ReadLine();
                            if (respuesta.Trim().ToUpper() != "S")
                            {
                                continue; // Saltar este archivo si no se desea reemplazar
                            }
                        }

                        entrada.WriteToDirectory(carpetaDestino, new ExtractionOptions()
                        {
                            ExtractFullPath = true,
                            Overwrite = true // Reemplazar archivos existentes
                        });
                        Console.WriteLine($"Archivo descomprimido: {entrada.Key}");
                    }
                }
            }
            Console.WriteLine("Descompresión completada.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al descomprimir el archivo RAR: {ex.Message}");
        }
    }
}

Hhhhhhhh

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main(string[] args)
    {
        string archivoRAR = "tu_archivo.rar";
        string directorioDestino = "directorio_destino";

        try
        {
            // Validar que el archivo RAR exista
            if (!File.Exists(archivoRAR))
            {
                Console.WriteLine("El archivo RAR no existe.");
                return;
            }

            // Crear el directorio de destino si no existe
            if (!Directory.Exists(directorioDestino))
            {
                Directory.CreateDirectory(directorioDestino);
            }

            // Descomprimir el archivo RAR
            using (var archive = ArchiveFactory.Open(archivoRAR))
            {
                foreach (var entry in archive.Entries)
                {
                    if (!entry.IsDirectory)
                    {
                        entry.WriteToDirectory(directorioDestino, new ExtractionOptions()
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
            Console.WriteLine("Error al descomprimir el archivo RAR: " + ex.Message);
        }
    }
}


4444
using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main(string[] args)
    {
        string archivoRAR = "archivo.rar";
        string destino = "carpeta_destino";

        try
        {
            using (var stream = File.OpenRead(archivoRAR))
            {
                using (var reader = ReaderFactory.Open(stream))
                {
                    while (reader.MoveToNextEntry())
                    {
                        if (!reader.Entry.IsDirectory)
                        {
                            string filePath = Path.Combine(destino, reader.Entry.Key);

                            // Verificar si el archivo ya existe
                            if (File.Exists(filePath))
                            {
                                Console.WriteLine($"El archivo {filePath} ya existe. ¿Deseas reemplazarlo? (S/N)");
                                string respuesta = Console.ReadLine();

                                if (respuesta.Trim().ToUpper() != "S")
                                    continue; // Si no se desea reemplazar, saltar al siguiente archivo
                            }

                            reader.WriteEntryToDirectory(destino, new ExtractionOptions()
                            {
                                ExtractFullPath = true,
                                Overwrite = true // Esto reemplazará el archivo si ya existe
                            });
                            Console.WriteLine($"Archivo descomprimido: {filePath}");
                        }
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

Mmmmmmmm

using System;
using System.IO;

class Program
{
    static void Main()
    {
        string origen = @"C:\ruta\carpeta_origen";
        string destino = @"C:\ruta\carpeta_destino";

        // Verificar si la carpeta de destino existe
        if (Directory.Exists(destino))
        {
            Console.WriteLine("La carpeta de destino ya existe. ¿Desea reemplazarla? (S/N)");

            // Leer la respuesta del usuario
            string respuesta = Console.ReadLine();

            if (respuesta.Equals("S", StringComparison.OrdinalIgnoreCase))
            {
                // Eliminar la carpeta de destino existente
                Directory.Delete(destino, true);
            }
            else
            {
                Console.WriteLine("Operación cancelada.");
                return;
            }
        }

        // Copiar la carpeta de origen a la carpeta de destino
        Directory.CreateDirectory(destino);
        CopiarDirectorio(origen, destino);

        Console.WriteLine("Carpeta copiada exitosamente.");
    }

    // Función para copiar un directorio y su contenido
    static void CopiarDirectorio(string origen, string destino)
    {
        foreach (string archivo in Directory.GetFiles(origen))
        {
            string archivoDestino = Path.Combine(destino, Path.GetFileName(archivo));
            File.Copy(archivo, archivoDestino);
        }

        foreach (string subdirectorio in Directory.GetDirectories(origen))
        {
            string subdirectorioDestino = Path.Combine(destino, Path.GetFileName(subdirectorio));
            CopiarDirectorio(subdirectorio, subdirectorioDestino);
        }
    }
}Este código copiará una carpeta



using System;
using System.IO;

class Program
{
    static void Main()
    {
        string origenDirectorio = @"C:\Ruta\DirectorioOrigen";
        string destinoDirectorio = @"C:\Ruta\DirectorioDestino";

        if (!Directory.Exists(origenDirectorio) || !Directory.Exists(destinoDirectorio))
        {
            Console.WriteLine("El directorio de origen o destino no existe.");
            return;
        }

        string[] archivos = Directory.GetFiles(origenDirectorio);

        foreach (string archivo in archivos)
        {
            string nombreArchivo = Path.GetFileName(archivo);
            string destinoArchivo = Path.Combine(destinoDirectorio, nombreArchivo);

            if (File.Exists(destinoArchivo))
            {
                Console.WriteLine($"¿Desea reemplazar '{nombreArchivo}'? (S/N)");
                string respuesta = Console.ReadLine();

                if (respuesta.Trim().Equals("S", StringComparison.OrdinalIgnoreCase))
                {
                    File.Copy(archivo, destinoArchivo, true);
                    Console.WriteLine($"'{nombreArchivo}' reemplazado.");
                }
                else
                {
                    Console.WriteLine($"'{nombreArchivo}' no fue reemplazado.");
                }
            }
            else
            {
                File.Copy(archivo, destinoArchivo);
                Console.WriteLine($"'{nombreArchivo}' copiado.");
            }
        }
    }
}

Nnnnnnn

using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main()
    {
        string sourceZipPath = "ruta_del_archivo.zip"; // Ruta del archivo ZIP que deseas descomprimir
        string extractPath = "ruta_de_la_carpeta_de_destino"; // Ruta de la carpeta de destino

        try
        {
            using (Stream stream = File.OpenRead(sourceZipPath))
            using (var reader = ReaderFactory.Open(stream))
            {
                while (reader.MoveToNextEntry())
                {
                    if (!reader.Entry.IsDirectory)
                    {
                        string fullPath = Path.Combine(extractPath, reader.Entry.Key);
                        
                        // Verificar si el archivo ya existe y preguntar si se desea reemplazar
                        if (File.Exists(fullPath))
                        {
                            Console.WriteLine($"El archivo '{fullPath}' ya existe. ¿Desea reemplazarlo? (S/N)");
                            string respuesta = Console.ReadLine();
                            if (respuesta.Trim().ToUpper() != "S")
                            {
                                continue; // Saltar este archivo si no se desea reemplazar
                            }
                        }

                        // Asegurarse de que el directorio de destino exista
                        Directory.CreateDirectory(Path.GetDirectoryName(fullPath));

                        // Extraer el archivo
                        using (var entryStream = reader.OpenEntryStream())
                        using (var output = File.Create(fullPath))
                        {
                            entryStream.CopyTo(output);
                        }

                        Console.WriteLine($"Archivo descomprimido: {fullPath}");
                    }
                }
            }

            Console.WriteLine("Descompresión completada.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al descomprimir: {ex.Message}");
        }
    }
}


using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main(string[] args)
    {
        string zipFilePath = "ruta_del_archivo.zip"; // Ruta del archivo ZIP
        string extractPath = "ruta_de_destino"; // Ruta donde deseas extraer los archivos

        using (var archive = ArchiveFactory.Open(zipFilePath))
        {
            foreach (var entry in archive.Entries)
            {
                // Construye la ruta completa para el archivo o carpeta a extraer
                string entryPath = Path.Combine(extractPath, entry.Key);

                if (entry.IsDirectory)
                {
                    // Si es una carpeta, asegúrate de que exista o pregúntale al usuario si debe reemplazarse
                    if (Directory.Exists(entryPath))
                    {
                        Console.WriteLine($"La carpeta ya existe: {entryPath}");
                        Console.Write("¿Desea reemplazarla? (S/N): ");
                        var response = Console.ReadLine();

                        if (response.Trim().ToUpper() == "S")
                        {
                            Directory.Delete(entryPath, true); // Elimina la carpeta existente y su contenido
                            Directory.CreateDirectory(entryPath); // Crea una nueva carpeta
                        }
                        else
                        {
                            continue; // Salta esta entrada si el usuario no desea reemplazarla
                        }
                    }
                    else
                    {
                        Directory.CreateDirectory(entryPath); // Crea la carpeta si no existe
                    }
                }
                else
                {
                    // Si es un archivo, asegúrate de que exista o pregúntale al usuario si debe reemplazarse
                    if (File.Exists(entryPath))
                    {
                        Console.WriteLine($"El archivo ya existe: {entryPath}");
                        Console.Write("¿Desea reemplazarlo? (S/N): ");
                        var response = Console.ReadLine();

                        if (response.Trim().ToUpper() != "S")
                        {
                            continue; // Salta esta entrada si el usuario no desea reemplazarla
                        }
                    }

                    entry.WriteToDirectory(extractPath, new ExtractionOptions
                    {
                        ExtractFullPath = true,
                        Overwrite = true
                    });
                }
            }
        }

        Console.WriteLine("La descompresión se ha completado con éxito.");
    }
}

B:bbbbbbbbbb

Comprimir un archivo en formato ZIP:using (var archive = ZipArchive.Create())
{
    archive.AddEntry("archivo.txt", "Contenido del archivo");
    archive.Save("archivo.zip");
}Comprimir una carpeta en formato ZIP:using (var archive = ZipArchive.Create())
{
    archive.AddAllFromDirectory("ruta_carpeta");
    archive.Save("carpeta.zip");
}Comprimir varias carpetas en un archivo ZIP:using (var archive = ZipArchive.Create())
{
    archive.AddAllFromDirectory("ruta_carpeta1");
    archive.AddAllFromDirectory("ruta_carpeta2");
    archive.Save("varias_carpetas.zip");
}Comprimir una carpeta y establecer una contraseña en el archivo ZIP:using (var archive = ZipArchive.Create())
{
    archive.AddAllFromDirectory("ruta_carpeta");
    archive.SaveTo("carpeta_con_contraseña.zip", new WriterOptions(CompressionType.Deflate)
    {
        Password = "tu_contraseña"
    });
}Comprimir un archivo en formato RAR:using (var archive = RarArchive.Create())
{
    archive.AddEntry("archivo.txt", "Contenido del archivo");
    archive.Save("archivo.rar");
}