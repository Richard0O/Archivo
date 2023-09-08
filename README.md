Descomprimir aquí (Extract Here):using (var stream = File.OpenRead("archivo.rar"))
{
    var reader = ReaderFactory.Open(stream);
    while (reader.MoveToNextEntry())
    {
        if (!reader.Entry.IsDirectory)
        {
            reader.WriteEntryToDirectory(".", ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
        }
    }
}Descomprimir creando una carpeta (Extract to Folder):using (var stream = File.OpenRead("archivo.rar"))
{
    var reader = ReaderFactory.Open(stream);
    while (reader.MoveToNextEntry())
    {
        if (!reader.Entry.IsDirectory)
        {
            var extractionPath = Path.Combine("carpeta_destino", reader.Entry.Key);
            reader.WriteEntryToDirectory(extractionPath, ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
        }
    }
}Descomprimir varios archivos (Extract Specific Files):using (var stream = File.OpenRead("archivo.rar"))
{
    var reader = ReaderFactory.Open(stream);
    while (reader.MoveToNextEntry())
    {
        if (!reader.Entry.IsDirectory)
        {
            // Agrega lógica para extraer solo los archivos que deseas aquí.
        }
    }
}Descomprimir archivos RAR con contraseña (Extract RAR Files with Password):using (var stream = File.OpenRead("archivo.rar"))
{
    var reader = ReaderFactory.Open(stream, new ReaderOptions() { Password = "tu_contraseña" });
    while (reader.MoveToNextEntry())
    {
        if (!reader.Entry.IsDirectory)
        {
            reader.WriteEntryToDirectory(".", ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
        }
    }
}

Ttttttt


using System;
using System.IO;
using System.Linq;
using SharpCompress.Common;
using SharpCompress.Readers;

class Program
{
    static void Main(string[] args)
    {
        string sourcePath = "ruta_del_archivo.rar"; // Ruta del archivo RAR
        string destinationFolder = "carpeta_destino"; // Carpeta de destino

        try
        {
            // Descomprimir el archivo RAR en la carpeta de destino
            ExtractRAR(sourcePath, destinationFolder);

            // Descomprimir varios archivos RAR en una carpeta
            string multipleSourcePath = "carpeta_con_varios_archivos_rar";
            string multipleDestinationFolder = "carpeta_destino_varios";
            ExtractMultipleRARs(multipleSourcePath, multipleDestinationFolder);

            // Descomprimir archivo RAR con contraseña
            string passwordProtectedRAR = "archivo_con_contraseña.rar";
            string password = "tu_contraseña";
            string passwordProtectedDestination = "carpeta_destino_contraseña";
            ExtractRARWithPassword(passwordProtectedRAR, password, passwordProtectedDestination);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }

    // Método para descomprimir un archivo RAR en una carpeta específica
    static void ExtractRAR(string sourcePath, string destinationFolder)
    {
        using (var stream = File.OpenRead(sourcePath))
        using (var reader = ReaderFactory.Open(stream))
        {
            while (reader.MoveToNextEntry())
            {
                if (!reader.Entry.IsDirectory)
                {
                    reader.WriteEntryToDirectory(destinationFolder, ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
                }
            }
        }
    }

    // Método para descomprimir varios archivos RAR en una carpeta
    static void ExtractMultipleRARs(string sourceFolderPath, string destinationFolder)
    {
        foreach (var sourcePath in Directory.GetFiles(sourceFolderPath, "*.rar"))
        {
            ExtractRAR(sourcePath, destinationFolder);
        }
    }

    // Método para descomprimir un archivo RAR protegido con contraseña
    static void ExtractRARWithPassword(string sourcePath, string password, string destinationFolder)
    {
        var options = new ExtractionOptions
        {
            Password = password
        };

        using (var stream = File.OpenRead(sourcePath))
        using (var reader = ReaderFactory.Open(stream, options))
        {
            while (reader.MoveToNextEntry())
            {
                if (!reader.Entry.IsDirectory)
                {
                    reader.WriteEntryToDirectory(destinationFolder, ExtractOptions.ExtractFullPath | ExtractOptions.Overwrite);
                }
            }
        }
    }
}