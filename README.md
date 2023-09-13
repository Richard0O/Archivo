Descomprimir aquí:using (var stream = File.OpenRead("archivo.rar"))
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