### PHP 5.5

Release: 20. Juni 2013

Damit nach [RFC:php53eol](https://wiki.php.net/rfc/php53eol) EndOfLife für PHP 5.3 am 20. Juni 2014!


#### Wichtige neue Sprach-Konstrukte die mit PHP 5.5 eingeführt wurden:

 * Generatoren
 * ```finally```
 * Password-API
 * ```list()``` in ```foreach```
 * beliebige Schlüssel in ```foreach```
 * ```::class```-Operator
 * dereferenzieren von Strings und Arrays
 * ```empty()``` als echte Funktion


### Generatoren

    class Grep {
        protected $fileHandle = null;
        protected $regExp     = null;
        
        public function __construct($fileName, $regExp) {
            $this->regExp = $regExp;
            $this->fileHandle = fopen($fileName, 'r');
        }
        
        public function __destruct() {
            fclose($this->fileHandle);
        }
        
        public function iterator() {
            while (false !== $line = fgets($this->fileHandle)) {
                if (preg_match('/' . $this->regexp . '/', $line) {
                    yield $line;
                }
            }
        }
    }
    
    $grep = new Grep('myFile', 'foo');
    $iterator = $grep->iterator();
    foreach ($iterator as $line {
        echo $line;
    }

Note:

Mit ```$iterator->send($foo);``` können Werte in den Generator injiziert werden.
Diese können dann mit ```$foo = yield;``` ausgelesen werden. 
Es kann nur ein Argument angegeben werden!


### ```finally```

    $tmpFile = tempnam('/tmp', '');
    $fileHandle = fopen($tmpFile, 'w+');
    try {
        fwrite($fileHandle, $getContent());
    } catch (Exception $e) {
        error_log($e->getMEssage());
    } finally {
        fclose($fileHandle);
    }


### Password-API

    string  password_hash(string $password, integer $algorithm [, array $options]);
    array   password_get_info(string $hash);
    boolean password_verify(string $password, string $hash);
    boolean password_needs_rehash(string $hash, string $algorithm [, array $options);


### ```list()``` in ```foreach```

    $array = [
        ['a', 'b', 'c'],
        ['d', 'e', 'f'],
        ['g', 'h', 'i'],
    ]
    foreach ($array as list($one, $two, $three)) {
        echo $one . '::' . $two . '::' . $three . "\n";
    }
    
    // a::b::c
    // d::e::f
    // g::h::i


### beliebige Schlüssel in ```foreach```

    $array = (
        1.0 => '1.0',
        1.1 => '1.1',
        new StdClass() => 'new StdClass',
    )
    
    foreach ($array as $key => $item) {
        print_r($key);
    }


### ```::class``` - Operator

    class Foo() {
        public static function whoAmI() {
            echo self::class;
            echo parent::class;
            echo static::class;
        }
    }
    
    class Bar extends Foo {}
    
    Foo::whoAmI();
    Bar::whoAmI();


### dereferenzieren von Strings und Arrays

    function getArray() {
        return array('a', 'b', 'c');
    }
    function getDefaultPassword() {
        return 'test1234';
    }
    
    echo getArray()[2]; // b
    echo getDefaultPassword()[4]; // 1 


### ```empty``` als echte Funktion

    if (empty(trim($string)) {
        echo 'empty';
    }
