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
