////////////////////////////////////////////////
Comprimir carpetas 
using System;
using System.Collections.Generic;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;
using SharpCompress.Writers;

class Program
{
    static void Main()
    {
        List<string> archivosYCarpetasAComprimir = new List<string>
{
    @"C:\Users\ricky\Desktop\ArchivosTest\CarpetaTest1",
    @"C:\Users\ricky\Desktop\ArchivosTest\CarpetaTest"
    
    // Agrega más archivos y carpetas según sea necesario.
};
string archivoZipSalida = @"C:\Users\ricky\Desktop\ArchivosTest\archivoComprimido.zip";

        using (var archivoZip = File.OpenWrite(archivoZipSalida))
        {
            using (var writer = WriterFactory.Open(archivoZip, ArchiveType.Zip, CompressionType.Deflate))
            {
                foreach (var rutaArchivoCarpeta in archivosYCarpetasAComprimir)
                {
                    if (File.Exists(rutaArchivoCarpeta))
                    {
                        writer.Write(Path.GetFileName(rutaArchivoCarpeta), File.OpenRead(rutaArchivoCarpeta), null);
                    }
                    else if (Directory.Exists(rutaArchivoCarpeta))
                    {
                        var archivosEnCarpeta = Directory.GetFiles(rutaArchivoCarpeta, "*", SearchOption.AllDirectories);
                        foreach (var archivo in archivosEnCarpeta)
                        {
                            var archivoRelativo = Path.GetRelativePath(rutaArchivoCarpeta, archivo);
                            writer.Write(archivoRelativo, File.OpenRead(archivo), null);
                        }
                    }
                    else
                    {
                        Console.WriteLine($"El archivo o carpeta '{rutaArchivoCarpeta}' no existe.");
                    }
                }
            }
        }

        Console.WriteLine("Compresión completada.");
    }
}
//////////////////////////////////////////////////
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main(string[] args)
    {
        // Ruta de la carpeta que deseas comprimir
        string sourceFolderPath = @"C:\ruta\a\tu\carpeta";

        // Ruta donde se guardará el archivo comprimido
        string destinationPath = @"C:\ruta\del\archivo\comprimido.zip";

        // Lista de rutas de archivos y carpetas que deseas comprimir
        List<string> itemsToCompress = new List<string>
        {
            "archivo1.txt",
            "subcarpeta1",
            "subcarpeta2/archivo2.txt"
        };

        // Crear el archivo comprimido
        CreateArchive(sourceFolderPath, destinationPath, itemsToCompress);

        Console.WriteLine("¡Carpeta comprimida con éxito!");
    }

    static void CreateArchive(string sourceFolderPath, string destinationPath, List<string> itemsToCompress)
    {
        using (var archive = ArchiveFactory.Create(ArchiveType.Zip, destinationPath))
        {
            foreach (var item in itemsToCompress)
            {
                string fullPath = Path.Combine(sourceFolderPath, item);
                if (File.Exists(fullPath))
                {
                    archive.AddEntry(item, fullPath);
                }
                else if (Directory.Exists(fullPath))
                {
                    // Si es una carpeta, agregar todos los archivos dentro de ella
                    var files = Directory.GetFiles(fullPath, "*", SearchOption.AllDirectories);
                    foreach (var file in files)
                    {
                        string relativePath = file.Substring(sourceFolderPath.Length + 1);
                        archive.AddEntry(relativePath, file);
                    }
                }
                else
                {
                    Console.WriteLine($"El archivo o carpeta '{item}' no existe en la ubicación especificada.");
                }
            }
        }
    }
}

//////////////////////////////////////////
Archivos de lista comprimidos
using System;
using System.Collections.Generic;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main(string[] args)
    {
        string archivoComprimido = @"C:\Users\ricky\Desktop\ArchivosTest\archivos.zip";
        List<string> archivosParaComprimir = new List<string>
{
    @"C:\Users\ricky\Desktop\ArchivosTest\ArchivoTest2.docx",
    @"C:\Users\ricky\Desktop\ArchivosTest\ArchivoTest.docx"
    
};

        using (var archivoSalida = File.OpenWrite(archivoComprimido))
        {
            using (var archivo = SharpCompress.Archives.Zip.ZipArchive.Create())
            {
                foreach (var archivoParaComprimir in archivosParaComprimir)
                {
                    archivo.AddEntry(Path.GetFileName(archivoParaComprimir), archivoParaComprimir);
                }
                archivo.SaveTo(archivoSalida, CompressionType.Deflate);
            }
        }

    }
}

//////////////////////////////////////////////////////////////////////
using System;
using System.IO;
using SharpCompress.Common;
using SharpCompress.Writers;

class Program
{
    static void Main(string[] args)
    {
        // Ruta de la carpeta que deseas comprimir
        string carpetaACoprimir = @"C:\Ruta\De\Tu\Carpeta";

        // Solicitar al usuario la ruta del archivo ZIP o usar el nombre de la carpeta
        Console.WriteLine("Especifica la ruta del archivo ZIP (deja en blanco para usar el nombre de la carpeta):");
        string rutaArchivoZip = Console.ReadLine();

        // Si el usuario no especifica una ruta, usa el nombre de la carpeta
        if (string.IsNullOrWhiteSpace(rutaArchivoZip))
        {
            rutaArchivoZip = Path.Combine(
                Path.GetDirectoryName(carpetaACoprimir),
                Path.GetFileNameWithoutExtension(carpetaACoprimir) + ".zip"
            );
        }

        // Comprime la carpeta en el archivo ZIP
        ComprimirCarpeta(carpetaACoprimir, rutaArchivoZip);

        Console.WriteLine($"Carpeta comprimida en: {rutaArchivoZip}");
    }

    static void ComprimirCarpeta(string carpetaOrigen, string archivoDestino)
    {
        using (var stream = new FileStream(archivoDestino, FileMode.Create))
        using (var writer = WriterFactory.Open(stream, ArchiveType.Zip, new WriterOptions(CompressionType.Deflate)))
        {
            writer.WriteAll(carpetaOrigen, "*", SearchOption.AllDirectories);
        }
    }
}
Jjjnn

using SharpCompress.Common;
using SharpCompress.Archives;
using SharpCompress.Writers;
using SharpCompress.Readers;

// Comprimir un archivo con contraseña
using (var archive = ArchiveFactory.Create(ArchiveType.Zip))
{
    archive.AddEntry("archivo.txt", "ruta/al/archivo.txt");

    // Establecer la contraseña
    archive.SaveTo("archivo_con_contraseña.zip", new WriterOptions(CompressionType.Deflate)
    {
        Password = "tu_contraseña_secreta"
    });
}

// Descomprimir un archivo con contraseña
using (var archive = ArchiveFactory.Open("archivo_con_contraseña.zip", new ReaderOptions()
{
    Password = "tu_contraseña_secreta"
}))
{
    foreach (var entry in archive.Entries)
    {
        if (!entry.IsDirectory)
        {
            entry.WriteToDirectory("directorio_de_destino", new ExtractionOptions()
            {
                ExtractFullPath = true,
                Overwrite = true
            });
        }
    }
}

////////

using Aspose.Zip;

class Program
{
    static void Main(string[] args)
    {
        // Ruta al archivo ZIP que deseas extraer
        string archivoZip = "ruta/al/archivo.zip";

        // Configura el objeto de extracción
        using (var archive = new Archive(archivoZip))
        {
            // Extrae todos los archivos en la misma ubicación
            archive.ExtractToDirectory(".");
        }

        Console.WriteLine("Archivos extraídos correctamente.");
    }
}

///////

using Aspose.Zip;

class Program
{
    static void Main(string[] args)
    {
        // Ruta del archivo ZIP que deseas extraer
        string archivoZip = "ruta_del_archivo.zip";

        // Crear una instancia de ZipArchive para abrir el archivo ZIP
        using (ZipArchive zipArchive = new ZipArchive(archivoZip))
        {
            // Obtener el nombre del archivo sin extensión
            string nombreCarpeta = System.IO.Path.GetFileNameWithoutExtension(archivoZip);

            // Crear una carpeta con el nombre del archivo ZIP
            System.IO.Directory.CreateDirectory(nombreCarpeta);

            // Extraer el contenido del archivo ZIP en la carpeta creada
            zipArchive.ExtractToDirectory(nombreCarpeta);
        }

        Console.WriteLine("Archivo ZIP extraído exitosamente.");
    }
}

Nnnmmn

// Ruta del archivo ZIP que deseas crear o sobrescribir
string zipFilePath = "ruta/del/archivo.zip";

// Verifica si el archivo ZIP ya existe
if (File.Exists(zipFilePath))
{
    // Pregunta al usuario si desea sobrescribir el archivo
    Console.Write("El archivo ZIP ya existe. ¿Deseas sobrescribirlo? (S/N): ");
    string respuesta = Console.ReadLine();

    // Si el usuario responde "S" o "s", sobrescribe el archivo; de lo contrario, sal del programa
    if (respuesta.Equals("S", StringComparison.OrdinalIgnoreCase))
    {
        // Crea un nuevo archivo ZIP o sobrescribe el existente
        using (var archive = new Archive(zipFilePath))
        {
            // Agrega tus listas u otros archivos al archivo ZIP
            // Por ejemplo:
            // archive.CreateEntry("lista1.txt", File.OpenRead("ruta/a/lista1.txt"));
            // archive.CreateEntry("lista2.txt", File.OpenRead("ruta/a/lista2.txt"));
            // ...

            Console.WriteLine("Archivo ZIP sobrescrito exitosamente.");
        }
    }
    else
    {
        Console.WriteLine("No se ha realizado ninguna acción.");
    }
}
else
{
    // El archivo ZIP no existe, por lo que puedes crearlo sin preguntar
    using (var archive = new Archive(zipFilePath))
    {
        // Agrega tus listas u otros archivos al archivo ZIP
        // Por ejemplo:
        // archive.CreateEntry("lista1.txt", File.OpenRead("ruta/a/lista1.txt"));
        // archive.CreateEntry("lista2.txt", File.OpenRead("ruta/a/lista2.txt"));
        // ...

        Console.WriteLine("Archivo ZIP creado exitosamente.");
    }
}

// Espera a que el usuario presione una tecla antes de salir
Console.ReadKey();