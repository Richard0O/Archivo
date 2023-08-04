using System;
using System.Collections.Generic;
using System.IO;
using System.IO.Compression;

class Program
{
    static void Main()
    {
        string zipFilePath = "archivo.zip";
        string extractionPath = "extracted_files";
        string txtFilePath = "archivos.txt";
        string finalZipPath = "archivos_comprimidos.zip";

        ExtractZipFile(zipFilePath, extractionPath);
        List<string> fileList = GetFileList(extractionPath);
        ShowFileList(fileList);
        ConvertToTxt(fileList, txtFilePath);
        CompressToZip(txtFilePath, finalZipPath);

        Console.WriteLine("Proceso completado.");
    }

    static void ExtractZipFile(string zipFilePath, string extractionPath)
    {
        if (!Directory.Exists(extractionPath))
        {
            ZipFile.ExtractToDirectory(zipFilePath, extractionPath);
        }
    }

    static List<string> GetFileList(string folderPath)
    {
        List<string> fileList = new List<string>();
        fileList.AddRange(Directory.GetFiles(folderPath));
        return fileList;
    }

    static void ShowFileList(List<string> fileList)
    {
        Console.WriteLine("Archivos extraídos:");
        foreach (string file in fileList)
        {
            Console.WriteLine(file);
        }
    }

    static void ConvertToTxt(List<string> fileList, string txtFilePath)
    {
        using (StreamWriter writer = new StreamWriter(txtFilePath))
        {
            foreach (string file in fileList)
            {
                string content = File.ReadAllText(file);
                writer.WriteLine($"Contenido de {Path.GetFileName(file)}:");
                writer.WriteLine(content);
                writer.WriteLine();
            }
        }
    }

    static void CompressToZip(string txtFilePath, string finalZipPath)
    {
        string parentFolder = Path.GetDirectoryName(txtFilePath);
        string[] filesToZip = Directory.GetFiles(parentFolder, "*.txt");
        ZipFile.CreateFromDirectory(parentFolder, finalZipPath);
    }
}
