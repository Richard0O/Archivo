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