
Comprimir carpetas y archivos Aqui.
try
        {
            using (Archive archive = new Archive(archivoZip))
            {
                foreach (var entry in archive.Entries)
                {
                    if (entry.IsDirectory)
                    {
                        // Si el archivo dentro del ZIP es un directorio, puedes crear la carpeta correspondiente
                        string carpetaDestinoCompleta = Path.Combine(Path.GetDirectoryName(archivoZip), entry.Name);
                        Directory.CreateDirectory(carpetaDestinoCompleta);
                    }
                    else
                    {
                        // Si el archivo dentro del ZIP es un archivo, extraerlo
                        string rutaArchivoDestino = Path.Combine(Path.GetDirectoryName(archivoZip), entry.Name);

                        if (!File.Exists(rutaArchivoDestino))
                        {
                            entry.Extract(rutaArchivoDestino);
                            Console.WriteLine($"Archivo extraído: {rutaArchivoDestino}");
                        }
                        else
                        {
                            Console.WriteLine($"El archivo ya existe: {rutaArchivoDestino}");
                        }
                    }
                }
            }
            Console.WriteLine("Proceso completado.");
        }
        catch (Exception)
        {
            throw;
        }
//////////////////////////////////////////////////////////////
Etraer un archivo zip (Extraer en)

        // Ruta del archivo ZIP que quieres extraer
        string archivoZip = @"C:\Users\ricky\Desktop\ArchivosTest/ArchivosTest.zip";

        // Directorio de destino donde se extraerán los archivos
        string directorioDestino = @"C:\Users\ricky\Desktop\ArchivosTest";


        // Crear una instancia de la clase ZipArchive
        using (var zipArchive = new Archive(archivoZip))
        {
            // Obtiene el nombre del archivo ZIP sin la extensión
            string nombreCarpeta = Path.GetFileNameWithoutExtension(archivoZip);

            // Crea la carpeta de destino si no existe
            string carpetaDestino = Path.Combine(directorioDestino, nombreCarpeta);
            Directory.CreateDirectory(carpetaDestino);

            // Extrae los archivos del ZIP en la carpeta de destino
            zipArchive.ExtractToDirectory(carpetaDestino);
        }
  //////////////////////////////////////////////////

Comprimir archivo con o sin ruta 
 // Verificar si la ruta de destino está en blanco
        if (string.IsNullOrWhiteSpace(rutaDestino))
        {
            // Si la ruta de destino está en blanco, utiliza el nombre del archivo como nombre de destino
            rutaDestino = Path.Combine(
                Path.GetDirectoryName(archivoOrigen),
                Path.GetFileNameWithoutExtension(archivoOrigen) + ".zip"
            );
        }
        // Crear FileStream para el archivo ZIP de salida
        using (FileStream zipFile = File.Open(rutaDestino, FileMode.Create))
        {
            // Archivo que se agregará al archivo
            using (FileStream source1 = File.Open(archivoOrigen, FileMode.Open, FileAccess.Read))
            {
                using (var archive = new Archive(new ArchiveEntrySettings()))
                {
                    // Agregar archivo al archivo
                    archive.CreateEntry(archivoOrigen, source1);
                    // archivo zip
                    archive.Save(zipFile);
                }
            }
        }
/////////////////////////////////////////////////////////////
Comprimir Usnado listas

using (Archive zipArchive = new Archive())
        {
            // Lista de archivos que deseas comprimir
            List<string> archivosAComprimir = new List<string>
            {
                @"C:\Users\ricky\Desktop\ArchivosTest\ArchivoTest2.docx",
                @"C:\Users\ricky\Desktop\ArchivosTest\ArchivoTest.docx"
            };

            // Agregar archivos a la lista de archivos del archivo ZIP
            foreach (string archivo in archivosAComprimir)
            {
                zipArchive.CreateEntry(archivo,archivo);
            }

            // Nombre del archivo ZIP de salida
            string archivoZipSalida = @"C:\Users\ricky\Desktop\ArchivosTest\archivos_comprimidos.zip";

            // Guardar el archivo ZIP comprimido
            zipArchive.Save(archivoZipSalida);
        }

        Console.WriteLine("Archivos comprimidos exitosamente.");

////////////////////////////////////////////////////////////

Comprimir Una carpeta

       if (string.IsNullOrWhiteSpace(archivoDestino))
        {
            archivoDestino = Path.Combine(Path.GetDirectoryName(carpetaOrigen), Path.GetFileNameWithoutExtension(carpetaOrigen) + ".zip");
        }

        // Crear un objeto ZipArchive para comprimir la carpeta
        using (var zipArchive = new Archive())
        {
            // Agregar la carpeta al archivo ZIP
            zipArchive.CreateEntries(carpetaOrigen);

            // Guardar el archivo ZIP en la ruta de salida especificada
            zipArchive.Save(archivoDestino);
        }
////////////////////////////////////////////////////////////////
 List<string> carpetasAComprimir = new List<string>
        {
            @"C:\Users\ricky\Desktop\ArchivosTest\Nueva carpeta",
           @"C:\Users\ricky\Desktop\ArchivosTest\ArchivosTest",
            // Agrega más rutas de carpetas según tus necesidades
        };

        // Ruta del archivo ZIP de salida
        string archivoZipSalida = @"C:\Users\ricky\Desktop\ArchivosTest\resultado.zip";

        // Comprimir las carpetas en un archivo ZIP
        using (var archive = new Archive())
        {
            foreach (string carpeta in carpetasAComprimir)
            {
                // Agrega cada carpeta a la lista de archivos a comprimir
                archive.CreateEntries(carpeta);
            }

            // Guarda el archivo ZIP resultante
            archive.Save(archivoZipSalida);
        }

        Console.WriteLine("Carpetas comprimidas con éxito en " + archivoZipSalida);

        Console.WriteLine($"La carpeta '{carpetaOrigen}' ha sido comprimida en '{archivoDestino}'.");

////////////////////////////////////////////////////////
Comprimir vrias carpetas

 List<string> carpetasAComprimir = new List<string>
        {
            @"C:\Users\ricky\Desktop\ArchivosTest\Nueva carpeta",
           @"C:\Users\ricky\Desktop\ArchivosTest\ArchivosTest",
            // Agrega más rutas de carpetas según tus necesidades
        };

        // Ruta del archivo ZIP de salida
        string archivoZipSalida = @"C:\Users\ricky\Desktop\ArchivosTest\resultado.zip";

        // Comprimir las carpetas en un archivo ZIP
        using (var archive = new Archive())
        {
            foreach (string carpeta in carpetasAComprimir)
            {
                // Agrega cada carpeta a la lista de archivos a comprimir
                archive.CreateEntries(carpeta);
            }

            // Guarda el archivo ZIP resultante
            archive.Save(archivoZipSalida);
        }

        Console.WriteLine("Carpetas comprimidas con éxito en " + archivoZipSalida);

////////////////////////////////////////
        
 System.Text.Encoding.RegisterProvider(System.Text.CodePagesEncodingProvider.Instance);
        string archivoZip = @"C:\Users\ricky\Desktop\ArchivosTest\ArchivosTest.zip";

        // Contraseña que deseas validar
        string password = "123";

        //string archivoZip = "ruta/archivo.zip"; // Ruta del archivo ZIP
        string destino = @"C:\Users\ricky\Desktop\ArchivosTest"; // Directorio de destino donde se extraerán los archivos
                                                                 //string contraseña = "tu_contraseña"; // Contraseña del archivo ZIP (si está protegido con contraseña)


        try
        {
            // Verificar si el archivo ZIP existe
            if (!File.Exists(archivoZip))
            {
                Console.WriteLine("El archivo ZIP no existe.");
                return;
            }

            // Crear un objeto ZipArchive para abrir el archivo ZIP
            using (Archive zip = new Archive(archivoZip, new ArchiveLoadOptions { DecryptionPassword = password }))
            {
                // Establecer la contraseña si el archivo ZIP está protegido

                // Extraer todos los archivos y carpetas del archivo ZIP
                zip.ExtractToDirectory(destino); // El segundo parámetro indica que se deben mantener los subdirectorios

                Console.WriteLine("Archivos extraídos con éxito.");
            }

            // Verificar si los archivos extraídos ya existen en el directorio de destino
            foreach (string archivoExtraido in Directory.GetFiles(destino, "*", SearchOption.AllDirectories))
            {
                Console.WriteLine($"Archivo extraído: {archivoExtraido}");
            }

            Console.WriteLine("Proceso completado.");


        }
        catch (Exception ex)
        { 
            Console.WriteLine("Error " + ex.Message);
        }
    }

Llll
using Aspose.Zip;

class Program
{
    static void Main()
    {
        // Ruta del archivo RAR y contraseña (si es necesario)
        string archivoRAR = "ruta_del_archivo.rar";
        string contraseña = "tu_contraseña";

        // Ruta de la carpeta de destino
        string carpetaDestino = "ruta_de_la_carpeta_de_destino";

        // Inicializa el objeto de extracción
        using (var zipArchive = new ZipArchive(archivoRAR))
        {
            // Si hay contraseña, configúrala
            if (!string.IsNullOrEmpty(contraseña))
            {
                zipArchive.Password = contraseña;
            }

            // Extraer todos los archivos y subdirectorios
            zipArchive.ExtractToDirectory(carpetaDestino);

            // Verificar si se extrajeron archivos
            if (zipArchive.Entries.Count > 0)
            {
                Console.WriteLine("Archivos extraídos exitosamente.");
            }
            else
            {
                Console.WriteLine("No se encontraron archivos en el archivo RAR.");
            }
        }
    }
}


Fffffgcgg

using Aspose.Zip;
using System;

class Program
{
    static void Main(string[] args)
    {
        // Ruta del archivo ZIP
        string zipFilePath = "ruta/al/archivo.zip";

        // Ruta de la carpeta de destino
        string extractFolderPath = "ruta/de/destino";

        // Contraseña del archivo ZIP (ajusta esto según tu caso)
        string zipPassword = "tu_contraseña";

        // Verificar la contraseña antes de extraer
        using (var archive = new Archive(zipFilePath))
        {
            if (archive.Password != zipPassword)
            {
                Console.WriteLine("Contraseña incorrecta para el archivo ZIP.");
                return; // Salir sin extraer si la contraseña es incorrecta
            }

            foreach (var entry in archive.Entries)
            {
                if (!entry.IsDirectory)
                {
                    // Ruta completa del archivo extraído
                    string extractedFilePath = Path.Combine(extractFolderPath, entry.Name);

                    // Verificar si el archivo ya existe
                    if (File.Exists(extractedFilePath))
                    {
                        Console.WriteLine($"El archivo {entry.Name} ya existe.");
                    }
                    else
                    {
                        entry.Extract(extractFolderPath);
                        Console.WriteLine($"El archivo {entry.Name} se extrajo con éxito.");
                    }
                }
            }
        }
    }
}


Hhhh

class Program
{
    static void Main(string[] args)
    {
        string zipFilePath = "archivo.zip"; // Ruta al archivo ZIP
        string extractPath = "carpeta_extraida"; // Ruta donde deseas extraer los archivos

        using (var archive = new Archive(zipFilePath))
        {
            foreach (var entry in archive.Entries)
            {
                if (entry.IsFolder)
                {
                    // Verificar si la entrada es una carpeta y crearla en la ubicación de destino
                    string folderPath = System.IO.Path.Combine(extractPath, entry.Name);
                    if (!System.IO.Directory.Exists(folderPath))
                    {
                        System.IO.Directory.CreateDirectory(folderPath);
                    }
                }
                else
                {
                    // Verificar si el archivo ya existe en la ubicación de destino
                    string filePath = System.IO.Path.Combine(extractPath, entry.Name);
                    if (!System.IO.File.Exists(filePath))
                    {
                        // Si el archivo no existe, extraerlo
                        entry.Extract(filePath);
                    }
                    else
                    {
                        // El archivo ya existe, puedes manejar esto según tus necesidades
                        Console.WriteLine($"El archivo {entry.Name} ya existe en {extractPath}");
                    }
                }
            }
        }
    }
}


Hhhhhbh

using Aspose.Zip;

class Program
{
    static void Main()
    {
        // Ruta al archivo ZIP que deseas extraer
        string zipFilePath = "ruta/al/archivo.zip";
        
        // Crear una instancia de ZipArchive para trabajar con el archivo ZIP
        using (var zipArchive = new ZipArchive(zipFilePath))
        {
            // Obtener el nombre del archivo ZIP sin la extensión
            string folderName = System.IO.Path.GetFileNameWithoutExtension(zipFilePath);
            
            // Crear una carpeta con el mismo nombre que el archivo ZIP
            System.IO.Directory.CreateDirectory(folderName);
            
            // Extraer todos los archivos y carpetas del archivo ZIP en la carpeta creada
            zipArchive.ExtractTo(folderName);
        }
    }
}

///////////////////////////

using System;
using System.IO;
using SharpCompress.Archives;
using SharpCompress.Common;

class Program
{
    static void Main()
    {
        string archivoZip = @"C:\Users\ricky\Desktop\ArchivosTest/ArchivosTest.zip"; // Reemplaza con el nombre de tu archivo ZIP


        string carpetaDestino = Path.Combine(Path.GetFileNameWithoutExtension(Path.GetDirectoryName(archivoZip))); // Obtiene el nombre sin extensión
        
        if (!Directory.Exists(carpetaDestino))
        {
            Directory.CreateDirectory(carpetaDestino); // Crea la carpeta de destino si no existe
        }

        using (var archive = ArchiveFactory.Open(archivoZip))
        {
            foreach (var entry in archive.Entries)
            {
                if (!entry.IsDirectory)
                {
                    entry.WriteToDirectory(carpetaDestino, new ExtractionOptions()
                    {
                        ExtractFullPath = true,
                        Overwrite = true
                    });
                }
            }
        }

        Console.WriteLine($"El archivo ZIP '{archivoZip}' se extrajo en la carpeta '{carpetaDestino}'.");
    }
}


Yyyyyy


               // Verifica si la contraseña es correcta
                bool contraseñaCorrecta = zipArchive.VerifyPassword(contraseña);


//////////

using Aspose.Zip;

class Program
{
    static void Main(string[] args)
    {
        string archivoZip = "tu_archivo.zip";
        string contraseña = "tu_contraseña";

        try
        {
            using (var zipArchive = new Archive(archivoZip))
            {
                // Verifica si la contraseña es correcta
                bool contraseñaCorrecta = zipArchive.VerifyPassword(contraseña);

                if (contraseñaCorrecta)
                {
                    Console.WriteLine("Contraseña correcta.");
                }
                else
                {
                    Console.WriteLine("Contraseña incorrecta.");
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error: " + ex.Message);
        }
    }
}

Rrrrrf

string archivoAComprimir = "ruta\\archivo-a-comprimir.txt"; // Reemplaza con la ruta de tu archivo
string archivoZIP = "ruta\\archivo-comprimido.zip"; // Reemplaza con la ruta de destino del archivo ZIP

using (var archive = new Archive())
{
    archive.CreateEntryFromFile(archivoAComprimir, Path.GetFileName(archivoAComprimir), CompressionType.Deflated);
    archive.Save(archivoZIP);
}

Ggggggvvg

List<string> archivosParaComprimir = new List<string>
{
    "archivo1.txt",
    "archivo2.png",
    // Agrega más archivos aquí
};

foreach (string archivo in archivosParaComprimir)
{
    archive.CreateEntryFromFile(archivo, Path.GetFileName(archivo), CompressionType.Deflated);
}Finalmente, guarda la lista de archivos comprimidos en un archivo ZIP:archive.Save("archivo_comprimido.zip"

Hhhhhhh

using (var archivoZip = new Archive())
{
    foreach (string carpeta in carpetasAComprimir)
    {
        archivoZip.CreateEntry(carpeta, "", true);
    }

    // Guarda el archivo ZIP en la ubicación deseada
    archivoZip.Save(@"C:\Ruta\De\Salida\archivo.zip");
}

Hhhhh

using Aspose.Zip;

class Program
{
    static void Main(string[] args)
    {
        // Ruta de la carpeta que deseas comprimir
        string carpetaOrigen = @"C:\Ruta\A\Tu\Carpeta";

        // Ruta del archivo zip de destino
        string archivoZip = @"C:\Ruta\Destino\Archivo.zip";

        // Crear una instancia de ZipArchive
        using (var archive = new ZipArchive())
        {
            // Agregar la carpeta a comprimir al archivo zip
            archive.CreateEntry(carpetaOrigen, "Carpeta");

            // Guardar el archivo zip
            archive.Save(archivoZip);
        }
    }
}

//// Angular 

//////////////////////////////////////// Css.

{ margin: 0; padding: 0; box-sizing: border-box; }

.form-register { width: 400px; background: #476786; padding: 30px; margin: auto; margin-top: 100px; border-radius: 4px; font-family: 'calibri'; color: white;

}

.form-register h2 { font-size: 24px; margin-bottom: 20px;

}

.inputs { width: 100%; background: #24303c; padding: 10px; border-radius: 4px; margin-bottom: 16px; border: 1px solid #476786; font-family: 'calibri'; font-size: 18px; color: white; }

.form-register label { width: 100%; background: #476786; padding: 10px; font-family: 'calibri'; font-size: 18px; color: rgb(19, 13, 13); }

.form-register button { width: 100%; /* background: #1f53c5; */ border: #24303c; padding: 12px; color: #24303c;; margin: 16px 0; font-size: 16px; }

.cajaSpan{

width: 400px; background: #24303c; padding: auto; margin: auto; margin-top: 100px; border-radius: 4px; font-family: 'calibri'; color: white; }

/////////////////////////////////////////////////////////// Archivo.html

formulario works!

Formulario de registro 
Nombre:
    <div class="form-group">
        <label for="correo">Corre electrónico:</label>
        <input class="inputs" type="email" id="correo" name="correo" required [(ngModel)]="correo"
            placeholder="Corre electrónico">
    </div>

    <div class="form-group">
        <label for="direccion">Dirección:</label>
        <input class="inputs" type="text" id="direccion" name="direccion" required [(ngModel)]="direccion"
            placeholder="Dirección">
    </div>

    <div class="form-group">
        <label for="pais">País:</label>
        <!-- <select class="inputs" id="pais" name="pais" required [(ngModel)]="pais"> -->
        <select class="inputs" id="pais" name="pais" required [(ngModel)]="pais">
            <option *ngFor="let pais of paises" [value]="pais.id">{{ pais.nombre }}</option>
        
        </select>
    </div>

    <div class="form-group">
        <label for="estado">Estado:</label>
        <select class="inputs" id="estado" name="estado" required [(ngModel)]="estado">
            <option *ngFor="let estados of estadosPorPais[pais]" [value]="estados">{{ estados }}</option>
        </select>
    </div>

    <button [disabled]="registroForm.invalid" (click)="onSubmit()">Registrar</button>
</form>
{{nombre}} - {{correo}} - {{direccion}} - {{pais}} - {{estado}}
///////////////////////////////////////////////////////////////////////////// Archivo.Ts

// registro = { // nombre: '', // apellido: '', // pais: '', // estado: '' // }; nombre: string = ""; correo: string = ""; direccion: string = ""; pais: string = ""; estado: string = "";

paises = [ { id: '1', nombre: 'México'}, { id: '2', nombre: 'Estados Unidos' }, { id: '3', nombre: 'Canadá'}, // Agrega más países y estados según sea necesario ];

estadosPorPais: { [key: string]: string[] } = { '1': ['Aguascalientes', 'Baja California', 'Baja California Sur', 'Campeche', 'Chiapas', 'Chihuahua', 'Ciudad de México', 'Coahuila', 'Colima', 'Durango', 'Estado de México', 'Guanajuato', 'Guerrero', 'Hidalgo', 'Jalisco', 'Michoacán', 'Morelos', 'Nayarit', 'Nuevo León', 'Oaxaca', 'Puebla', 'Querétaro', 'Quintana Roo', 'San Luis Potosí', 'Sinaloa', 'Sonora', 'Tabasco', 'Tamaulipas', 'Tlaxcala', 'Veracruz', 'Yucatán', 'Zacatecas' ], '2': ['Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California', 'Colorado', 'Connecticut', 'Delaware', 'Florida', 'Georgia', 'Hawaii', 'Idaho', 'Illinois', 'Indiana', 'Iowa', 'Kansas', 'Kentucky', 'Louisiana', 'Maine', 'Maryland', 'Massachusetts', 'Michigan', 'Minnesota', 'Mississippi', 'Missouri', 'Montana', 'Nebraska', 'Nevada', 'New Hampshire', 'New Jersey', 'New Mexico', 'New York', 'Carolina del Norte', 'Dakota del Norte', 'Ohio', 'Oklahoma', 'Oregon', 'Pennsylvania', 'Rhode Island', 'Carolina del Sur', 'Dakota del Sur', 'Tennessee', 'Texas', 'Utah', 'Vermont', 'Virginia', 'Washington', 'Virginia', 'Wisconsin' ], '3': ['Alberta', 'Columbia Británica', 'Manitoba', 'Nuevo Brunswick', 'Terranova y Labrador', 'Nueva Escocia', 'Ontario', 'Isla del Príncipe Eduardo', 'Quebec', 'Saskatchewan', 'Yukon', 'Nunavut', 'Territorios del Noroeste' ] };

onSubmit() {

  // Mostar datos en consola
  console.log('Datos del formulario:', this.nombre, this.correo, this.direccion, this.pais, this.estado);

  // Redirigir al usuario a una página de inicio de sesión o de agradecimiento
  // this.router.navigate(['/inicio-sesion']); // Asegúrate de importar RouterModule y ActivatedRoute para utilizar esta opción
}

shhshdkjlfñgh{kjñglfkdjgsf

using Aspose.Zip;

public class ZipHelper
{
    public void ComprimirArchivo(string archivoOrigen, string archivoDestino, string contraseña = null)
    {
        using (var archive = new Archive())
        {
            if (!string.IsNullOrEmpty(contraseña))
            {
                archive.Password = contraseña;
            }

            archive.CreateEntryFromFile(archivoOrigen, archivoDestino);
        }
    }

    public void ComprimirArchivos(string[] archivosOrigen, string archivoDestino, string contraseña = null)
    {
        using (var archive = new Archive())
        {
            if (!string.IsNullOrEmpty(contraseña))
            {
                archive.Password = contraseña;
            }

            foreach (var archivo in archivosOrigen)
            {
                archive.CreateEntryFromFile(archivo, Path.GetFileName(archivo));
            }

            archive.Save(archivoDestino);
        }
    }

    public bool ValidarContraseña(string archivoZIP, string contraseña)
    {
        using (var archive = new Archive(archivoZIP))
        {
            return archive.CheckPassword(contraseña);
        }
    }
}

////////////////////////////////////////7

class Principal
{
    public static void Main(string[] args)
    {
var zipHelper = new ZipHelper();

// Comprimir un solo archivo sin contraseña
zipHelper.ComprimirArchivo("archivo.txt", "archivo_sin_contraseña.zip");

// Comprimir un solo archivo con contraseña
zipHelper.ComprimirArchivo("archivo.txt", "archivo_con_contraseña.zip", "mi_contraseña");

// Comprimir varios archivos sin contraseña
string[] archivos = { "archivo1.txt", "archivo2.txt" };
zipHelper.ComprimirArchivos(archivos, "archivos_sin_contraseña.zip");

// Comprimir varios archivos con contraseña
zipHelper.ComprimirArchivos(archivos, "archivos_con_contraseña.zip", "mi_contraseña");

// Validar contraseña
bool esContraseñaCorrecta = zipHelper.ValidarContraseña("archivo_con_contraseña.zip", "mi_contraseña");
    }
}

