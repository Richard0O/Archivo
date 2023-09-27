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
using System.IO;
using System.Linq;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Writers;

class Program
{
    static void Main()
    {
        // Ruta del archivo ZIP a crear
        string zipFilePath = "archivo.zip";

        // Contraseña para el archivo ZIP
        string password = "tucontraseña";

        // Ruta de los archivos que deseas agregar al ZIP
        string[] filesToAdd = { "archivo1.txt", "archivo2.txt" };

        // Crear un archivo ZIP con contraseña
        using (var archiveStream = File.Create(zipFilePath))
        {
            // Crear un archivo ZIP protegido con contraseña
            using (var archive = WriterFactory.Open(archiveStream, ArchiveType.Zip, CompressionType.Deflate, new WriterOptions(CompressionType.Deflate) 
            { Password = password }))
            {
                foreach (var file in filesToAdd)
                {
                    // Agregar cada archivo a la entrada del archivo ZIP
                    var entry = archive.AddEntry(file, Path.GetDirectoryName(file));

                    // Lee el contenido del archivo y escribe en la entrada del archivo ZIP
                    using (var fileStream = File.OpenRead(file))
                    {
                        entry.WriteTo(fileStream);
                    }
                }
            }
        }

        Console.WriteLine("Archivo ZIP con contraseña creado con éxito.");
    }
}


        Console.WriteLine("Archivo comprimido en: " + rutaDeDestino);
    }
}
////////////////////////////////////////////

