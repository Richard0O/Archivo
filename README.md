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

Ttttttt

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

public void DescomprimirArchivo(string archivoRAR, string destino)
{
    using (var archive = ArchiveFactory.Open(archivoRAR))
    {
        foreach (var entry in archive.Entries)
        {
            if (!entry.IsDirectory)
            {
                string destinoArchivo = Path.Combine(destino, entry.Key);

                if (File.Exists(destinoArchivo))
                {
                    Console.WriteLine($"El archivo {entry.Key} ya existe. ¿Deseas reemplazarlo? (S/N)");
                    var respuesta = Console.ReadLine();
                    
                    if (respuesta.ToUpper() == "S")
                    {
                        entry.WriteTo(destinoArchivo, new ExtractionOptions()
                        {
                            ExtractFullPath = true,
                            Overwrite = true
                        });
                    }
                }
                else
                {
                    entry.WriteTo(destinoArchivo, new ExtractionOptions()
                    {
                        ExtractFullPath = true,
                        Overwrite = true
                    });
                }
            }
        }
    }ñññ


using (var archive = ArchiveFactory.Open("archivo.rar"))
{
    foreach (var entry in archive.Entries)
    {
        if (!entry.IsDirectory)
        {
            // Verifica si el archivo ya existe en la ubicación actual
            if (File.Exists(entry.Key))
            {
                // Si el archivo existe, puedes optar por sobrescribirlo o tomar otra acción
                // En este ejemplo, sobrescribiremos el archivo existente
                entry.WriteTo(entry.Key, new ExtractionOptions()
                {
                    ExtractFullPath = true,
                    Overwrite = true // Esto permite sobrescribir archivos existentes
                });
            }
            else
            {
                // Si el archivo no existe, simplemente extráelo
                entry.WriteTo(entry.Key, new ExtractionOptions()
                {
                    ExtractFullPath = true
                });
            }
        }
    }

////

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main()
    {
        string rarFilePath = "tu_archivo.rar"; // Reemplaza con la ubicación de tu archivo RAR
        string extractPath = "ruta_de_extracción"; // Reemplaza con la carpeta donde quieres extraer los archivos

        using (var archive = ArchiveFactory.Open(rarFilePath))
        {
            foreach (var entry in archive.Entries)
            {
                if (!entry.IsDirectory)
                {
                    string destinationPath = Path.Combine(extractPath, entry.Key);
                    
                    if (File.Exists(destinationPath))
                    {
                        // Puedes agregar lógica aquí para manejar la sobrescritura si es necesario
                        Console.WriteLine($"El archivo {entry.Key} ya existe. Sobrescribiendo...");
                        entry.WriteTo(destinationPath, new ExtractionOptions() { ExtractFullPath = true, Overwrite = true });
                    }
                    else
                    {
                        entry.WriteTo(destinationPath, new ExtractionOptions() { ExtractFullPath = true, Overwrite = false });
                    }
                }
            }
        }

        Console.WriteLine("Extracción completa.");
    }
}

Vvvvvv
string rutaArchivoRar = "ruta/del/archivo.rar";
string carpetaDestino = "ruta/de/la/carpeta/destino/";

using (Stream stream = File.OpenRead(rutaArchivoRar))
{
    using (var reader = ReaderFactory.Open(stream))
    {
        foreach (var entry in reader.Entries)
        {
            if (!entry.IsDirectory)
            {
                // Obtener el nombre del archivo del entry
                string nombreArchivo = Path.GetFileName(entry.Key);

                // Construir la ruta completa de destino con el mismo nombre
                string rutaDestino = Path.Combine(carpetaDestino, nombreArchivo);

                // Extraer el archivo en la carpeta de destino
                entry.WriteToDirectory(carpetaDestino, new ExtractionOptions()
                {
                    ExtractFullPath = true,
                    Overwrite = true // Puedes cambiar esto según tus necesidades
                });
            }
        }
    }
}

Ggggvv
using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Compressors;
using SharpCompress.Writers;

class Program
{
    static void Main()
    {
        string archivoAComprimir = "archivo.txt";
        string archivoComprimido = "archivo.rar";
        string contraseña = "tu_contraseña";

        using (var stream = File.OpenWrite(archivoComprimido))
        using (var writer = WriterFactory.Open(stream, ArchiveType.Rar, CompressionType.Deflate, new WriterOptions(CompressionType.Deflate)
        {
            Password = contraseña // Aquí estableces la contraseña
        }))
        {
            writer.Write(archivoAComprimir, Path.GetFileName(archivoAComprimir));
        }

        Console.WriteLine("Archivo comprimido con éxito.");
    }
}


Aaasassssa

using System;
using System.Diagnostics;

class Program
{
    static void Main()
    {
        // Specify the path to the WinRAR executable
        string winRarPath = @"C:\Program Files\WinRAR\WinRAR.exe";

        // Specify the source folder you want to compress
        string sourceFolder = @"C:\Path\To\Your\Folder";

        // Specify the destination archive file (including .rar extension)
        string destinationArchive = @"C:\Path\To\Your\Archive.rar";

        // Create a ProcessStartInfo object to configure the process
        ProcessStartInfo processInfo = new ProcessStartInfo
        {
            FileName = winRarPath,
            Arguments = $"a \"{destinationArchive}\" \"{sourceFolder}\"", // 'a' stands for 'add to archive'
            RedirectStandardOutput = true,
            RedirectStandardError = true,
            UseShellExecute = false,
            CreateNoWindow = true
        };

        // Create and start the process
        Process process = new Process
        {
            StartInfo = processInfo
        };

        process.Start();

        // Wait for the process to exit
        process.WaitForExit();

        // Check if the process exited successfully (ExitCode 0)
        if (process.ExitCode == 0)
        {
            Console.WriteLine("Compression successful.");
        }
        else
        {
            Console.WriteLine("Compression failed. Check for errors.");
        }

        // Close the process
        process.Close();
    }
}

Tttt

string archivoOrigen = "archivo.txt"; // Reemplaza con la ruta de tu archivo de origen
string archivoDestino = "archivo.rar"; // Nombre del archivo RAR resultante
string contraseña = "miContraseña"; // Reemplaza con tu contraseña

// Crear un archivo RAR con contraseña
using (var archivoSalida = File.OpenWrite(archivoDestino))
{
    using (var escritor = WriterFactory.Open(archivoSalida, ArchiveType.Rar, new WriterOptions(CompressionType.Rar, new SharpCompress.Common.CompressionInfo() { Password = contraseña })))
    {
        escritor.WriteAll(archivoOrigen, "carpetaDentroDelRAR");
    }
}
Jjjjjj

using System;
using System.Diagnostics;

class Program
{
    static void Main()
    {
        string winRarPath = @"C:\Program Files\WinRAR\WinRAR.exe"; // Ruta de WinRAR en tu sistema
        string archivoComprimido = @"C:\ruta\archivo.rar"; // Ruta del archivo comprimido
        string destinoDescomprimido = @"C:\ruta\destino"; // Carpeta de destino para la descompresión

        DescomprimirConWinRAR(winRarPath, archivoComprimido, destinoDescomprimido);
    }

    static void DescomprimirConWinRAR(string winRarPath, string archivoComprimido, string destinoDescomprimido)
    {
        ProcessStartInfo startInfo = new ProcessStartInfo();
        startInfo.FileName = winRarPath;
        startInfo.Arguments = $"x \"{archivoComprimido}\" \"{destinoDescomprimido}\""; // Argumentos para descomprimir

        Process process = new Process();
        process.StartInfo = startInfo;
        process.Start();

        process.WaitForExit(); // Esperar a que termine el proceso de descompresión

        Console.WriteLine("Descompresión completada.");
    }
}
ññññ
using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Writers;

class Program
{
    static void Main()
    {
        string archivoAComprimir = "archivo.txt";
        string rutaDestino = "archivo_comprimido.zip";
        string contraseña = "tu_contraseña";

        if (File.Exists(archivoAComprimir) && Directory.Exists(Path.GetDirectoryName(rutaDestino)))
        {
            using (var stream = new FileStream(rutaDestino, FileMode.Create))
            using (var writer = WriterFactory.Open(stream, ArchiveType.Zip, CompressionType.Deflate, new WriterOptions(CompressionType.Deflate) { ArchiveEncoding = new ArchiveEncoding() { UseUnicode = true }, Password = contraseña }))
            {
                writer.Write(Path.GetFileName(archivoAComprimir), archivoAComprimir);
            }

            Console.WriteLine("Archivo comprimido exitosamente.");
        }
        else
        {
            Console.WriteLine("El archivo o la ruta no existen.");
        }
    }
}