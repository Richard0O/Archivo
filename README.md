// Comprimir un archivo y ponerle el nombre del archivo a comprimir 

using SharpCompress.Common;
using SharpCompress.Writers;


class Program
{
    static void Main(string[] args)
    {
        // Ruta del archivo que se va a comprimir (proporcionada por el usuario)
        string archivoAComprimir = @"C:\Users\ricky\Desktop\ArchivosTest\ArchivoTest.docx";
        string archivoZip = "";
        
        if (File.Exists(archivoAComprimir))
        {
            if (string.IsNullOrWhiteSpace(archivoZip))
            {
                // Si no se proporciona una ruta para el archivo ZIP, se utiliza el nombre del archivo original
                archivoZip = Path.Combine(
                    Path.GetDirectoryName(archivoAComprimir),
                    Path.GetFileNameWithoutExtension(archivoAComprimir) + ".zip");
            }

            // Comprimir el archivo en un archivo ZIP
            using (var stream = File.OpenWrite(archivoZip))
            using (var writer = WriterFactory.Open(stream, ArchiveType.Zip, CompressionType.Deflate))
            {
                writer.Write(Path.GetFileName(archivoAComprimir), File.OpenRead(archivoAComprimir), null);
            }

            Console.WriteLine($"Archivo {archivoAComprimir} comprimido en {archivoZip}");
        }
        else
        {
            Console.WriteLine("El archivo especificado no existe.");
        }
    }
}

/////////////////////////////////////////////////////////////////77

Metodo para comprimir una carpeta 
using System;
using System.IO;
using System.IO.Compression;

class Program
{
    static void Main()
    {
        string carpetaOrigen = @"C:\Users\ricky\Desktop\ArchivosTest\ArchivoComprimir";

        string nombreArchivoDestino = "";

        // Comprobar si el usuario proporcionó un nombre de archivo ZIP, de lo contrario, usar el nombre de la carpeta
        if (string.IsNullOrWhiteSpace(nombreArchivoDestino))
        {
            nombreArchivoDestino = Path.GetFileName(carpetaOrigen) + ".zip";
        }
        else
        {
            // Asegurarse de que el nombre del archivo tenga la extensión .zip
            if (!nombreArchivoDestino.EndsWith(".zip", StringComparison.OrdinalIgnoreCase))
            {
                nombreArchivoDestino += ".zip";
            }
        }

        // Obtener la ruta completa del archivo ZIP de destino
        string rutaArchivoDestino = Path.Combine(Path.GetDirectoryName(carpetaOrigen), nombreArchivoDestino);

        try
        {
            // Comprimir la carpeta en el archivo ZIP
            ZipFile.CreateFromDirectory(carpetaOrigen, rutaArchivoDestino);
            Console.WriteLine($"La carpeta '{carpetaOrigen}' se ha comprimido en '{rutaArchivoDestino}'.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error al comprimir la carpeta: {ex.Message}");
        }
    }
}
////////////////////////////////////////////////7

Comprimir listas
public static void ComprimirCarpetasEnZip(List<string> carpetas, string archivoZip)
{
    // Crea un nuevo archivo ZIP
    using (var archivo = ZipArchive.Create())
    {
        // Configura las opciones de compresión
        var opciones = new WriterOptions(CompressionType.Deflate);
        
        foreach (var carpeta in carpetas)
        {
            if (Directory.Exists(carpeta))
            {
                // Agrega los archivos de la carpeta al archivo ZIP
                foreach (var archivoCarpeta in Directory.GetFiles(carpeta, "*", SearchOption.AllDirectories))
                {
                    var entrada = archivo.AddEntry(Path.GetRelativePath(carpeta, archivoCarpeta), archivoCarpeta, opciones);
                }
            }
            else
            {
                Console.WriteLine($"La carpeta '{carpeta}' no existe.");
            }
        }

        // Guarda el archivo ZIP en disco
        using (var stream = new FileStream(archivoZip, FileMode.Create))
        {
            archivo.SaveTo(stream, CompressionType.Deflate);
        }
    }
}

public static void Main(string[] args)
{
    List<string> carpetas = new List<string>
    {
        "ruta/carpeta1",
        "ruta/carpeta2",
        // Agrega más carpetas según sea necesario
    };

    string archivoZip = "ruta/archivo.zip";

    ComprimirCarpetasEnZip(carpetas, archivoZip);

    Console.WriteLine("Las carpetas se han comprimido en el archivo ZIP.");
}
////////////////////////////////////////////////


using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main()
    {
        // Lista de rutas de las carpetas que deseas comprimir
        List<string> carpetas = new List<string>
        {
            @"C:\Ruta\A\",
            @"C:\Ruta\B\"
        };

        // Ruta del archivo ZIP de salida
        string archivoZipSalida = @"C:\Ruta\archivo.zip";

        using (var stream = new FileStream(archivoZipSalida, FileMode.Create))
        using (var archive = ArchiveFactory.Create(ArchiveType.Zip, stream))
        {
            foreach (var carpeta in carpetas)
            {
                // Agrega cada archivo y carpeta de la ruta especificada al archivo ZIP
                foreach (var archivoOcarpeta in Directory.EnumerateFileSystemEntries(carpeta, "*", SearchOption.AllDirectories))
                {
                    string relativaPath = Path.GetRelativePath(carpeta, archivoOcarpeta);

                    if (File.Exists(archivoOcarpeta))
                    {
                        // Agrega archivo al ZIP
                        archive.AddEntry(relativaPath, File.ReadAllBytes(archivoOcarpeta));
                    }
                    else if (Directory.Exists(archivoOcarpeta))
                    {
                        // Agrega carpeta al ZIP
                        archive.AddEntry(relativaPath + Path.DirectorySeparatorChar, new byte[0]);
                    }
                }
            }
        }

        Console.WriteLine("Compresión completada.");
    }

/////////////////


using System;
using System.IO;
using System.Linq;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main()
    {
        // Ruta del archivo que deseas comprimir
        string archivoAComprimir = @"C:\Ruta\Archivo.txt";

        // Ruta de destino o deja en blanco para usar el nombre del archivo comprimido
        string rutaDeDestino = "";

        // Comprime el archivo
        ComprimirArchivo(archivoAComprimir, rutaDeDestino);
    }

    static void ComprimirArchivo(string archivoAComprimir, string rutaDeDestino)
    {
        // Obtiene el nombre del archivo sin la extensión
        string nombreArchivo = Path.GetFileNameWithoutExtension(archivoAComprimir);

        // Si la ruta de destino está en blanco, utiliza el nombre del archivo
        if (string.IsNullOrWhiteSpace(rutaDeDestino))
        {
            rutaDeDestino = nombreArchivo + ".zip";
        }
        else
        {
            // Combina la ruta de destino con el nombre del archivo
            rutaDeDestino = Path.Combine(rutaDeDestino, nombreArchivo + ".zip");
        }

        using (var archive = ArchiveFactory.Create(ArchiveType.Zip))
        {
            // Agrega el archivo al archivo comprimido con la estructura de carpetas
            archive.AddEntry(nombreArchivo + Path.GetExtension(archivoAComprimir),
                File.OpenRead(archivoAComprimir), false);
            archive.SaveTo(rutaDeDestino, CompressionType.Deflate);
        }

        Console.WriteLine("Archivo comprimido en: " + rutaDeDestino);
    }
}

