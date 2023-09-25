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
        Console.Write("Ruta de la carpeta a comprimir: ");
        string carpetaOrigen = Console.ReadLine();

        Console.Write("Nombre del archivo ZIP de destino (dejar en blanco para usar el nombre de la carpeta): ");
        string nombreArchivoDestino = Console.ReadLine();

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

