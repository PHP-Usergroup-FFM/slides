### PHP 5.4

Release: 1. März 2012

Note:
Neuer Release-Zyklus, RFC-Prozess


#### Wichtige neue Sprach-Konstrukte die mit PHP 5.4 eingeführt wurden:

 * Traits
 * Kurze Array-Notation
 * Array-Dereferenzierung
 * Zugriff auf Klassen-Member nach Instanzierung
 * Binäres Zahlen-Format
 * Fortschritt von Datei-Uploads über Session
 * eingebauter Webserver
 * Typehint ```callable```
 * Flexibilität bei variablen Methodenaufrufen
 * ```default_charset```-Voreinstellung


### Traits

    trait CountableTrait
    {
        abstract protected function getStorage();
    
        public function count() {
            return count($this->getStorage());
        }
    } 
    
    class TraitCounter implements \Countable
    {
        use CountableTrait;
        
        protected $storage = array();
        
        protected function &getStorage() {
            return $this->storage;
        }
        
        public function add($item) {
            $this->storage[] = $item;
            
            return $this;
        }
    }
    
    $counter = new TraitCounter();
    $counter->add('one')->add('two')->add('three');
    
    echo sprintf("Counter contains %s items\n", $counter->count());
 
Weiteres Beispiel auf [GitHub](https://github.com/heiglandreas/NewLanguageFeaturesIUntilPhp56/tree/master/OrgHeigl/NewLanguageFeatures/Php54/MyTrait).


### Kurze Array-Notation

    $f = ['a', 'b', 'c'];


### Array-Dereferenzierung für Funktionen

    function getArray() {
        return ['a', 'b', 'c'];
    }
    
    echo getArray()[2] // 'c'


### Zugriff auf Klassen-Member nach Instanziierung

    $bar = (new Foo())->getBar();


### Binäres Zahlen-Format

    $binary = 0b01001100
    echo $binary;
    // 76


### Fortschritt von Datei-Uploads über Sessions (1)

    <form action="upload.php" method="POST" enctype="multipart/form-data">
     <input type="hidden" name="<?php echo ini_get("session.upload_progress.name"); ?>" value="123" />
     <input type="file" name="file1" />
     <input type="file" name="file2" />
     <input type="submit" />
    </form>


### Fortschritt von Datei-Uploads über Sessions (2)

    $key = ini_get("session.upload_progress.prefix") . $_POST[ini_get("session.upload_progress.name")];
    var_dump($_SESSION[$key]);


### Fortschritt von Datei-Uploads über Sessions (3)

    array(
     "start_time" => 1234567890,   // Anfragezeitpunkt
     "content_length" => 57343257, // POST Gesamtdatenmenge (in Bytes)
     "bytes_processed" => 453489,  // Menge der empfangenen und verabreiteten Daten (in Bytes)
     "done" => false,              // true wenn die POST Datenverarbeitung beendet ist (erfolgreich oder nicht)
     "files" => array(
      0 => array(
       "field_name" => "file1",       // Name des <input/> Feldes
       // Die nächsten drei Elemente entsprechen den Angaben in $_FILES
       "name" => "foo.avi",
       "tmp_name" => "/tmp/phpxxxxxx",
       "error" => 0,
       "done" => true,                // true wenn die POST Datenverarbeitung dieser Datei beendet ist
       "start_time" => 1234567890,    // Zeitpunkt wann die Verarbeitung der Datei begonnen hat
       "bytes_processed" => 57343250, // Menge der empfangenen und verabreiteten Daten dieser Datei (in Bytes)
      ),
      // Eine weitere, noch nicht Komplett hochgeladene Datei, der selben Anfrage
      1 => array(
       "field_name" => "file2",
       "name" => "bar.avi",
       "tmp_name" => NULL,
       "error" => 0,
       "done" => false,
       "start_time" => 1234567899,
       "bytes_processed" => 54554,
      ),
     )
    );
    


### Eingebauter Webserver

    $ php -S 127.0.0.1:8088 -t /Users/Shared/Development/Sites


### Typehint ```callable```

    function execute(callable $callable) {
        if (iS_callable($callable) {
            $callable();
        }
    }


### Flexibilität bei variablen Methodenaufrufen

    $class  = new Foo();
    $props = ['width', 'height', 'top', 'left'];
    foreach($props as $prop) {
        $class->{'set' . $prop}(0);
    }


### ```default_charset``` - Voreinstellung

```default_charset``` steht standardmäßig auf **UTF-8** und nicht mehr auf **ISO-8859-1**

    
    
 