IDEA CONSISTENCY

~~IDEA 1- all types are constructors, and all of them accept a *name* as first parameter in order to *create & name* the value. Bad idea, makes "struct" inconsistent, because you must look ahead to see if it is a declaration or a value-instantiation.~~

~~IDEA 2- all storages have a name. Types-as-Constructors first parameter is the storage name, if omitted, an automatic-name is assigned.  Types are first-class-objects.~~

IDEA 3- **all storages have a name. Types-as-Constructors create values.  Types are first-class-objects.**

Some constructors create types, as "struct". It means "struct" is a level-1-type, and "Person" is a level-0-type. "Interface" is another level-1-type.

Other types have "parameters", so they are level-0 *with parameters*. i.e.: Table, Array, Number (decimal), Dictionary, String, Buffer, etc.

**Value= Literal | Expression**

**Let-Statement= "let" IDENTIFIER "=" Value**

**"let"** creates a constant-storage in the scope, initialized with the value.

    let Person = struct 
          field first-name string
          field last-name string

    print Person
    => (struct (field first-name string) (field last-name string))

    let oz = Person "Oscar" "Diggs"

**var-Statement= "var" IDENTIFIER [":" type-name] ["=" Value]**
Creates a variable-storage in the scope, initialized with the value.
if both specified, type-name must match (type-of Value)
if IDENTIFIER ends with a type-name, must match type-name and (type-of Value)

    var a-character = Person "Oscar" "Diggs"

    var favorite-character = oz

    var other-person //type:Person, initializes to zero-value

A *struct* initializes to zero-value.  a *struct* cannot be null. A *list* can be null (false, empty)

**Temp-Value= IDENTIFIER[:Type] "=" Value**
Creates a temporary value, to fill a parameter in a function call

   let oz = Person 
         first-name="Oscar"
         last-name="Diggs"

   let oz = Person (first-name="Oscar") (last-name="Diggs")

each parameter of a struct-type constructor is expected to be a value for a field

    print oz 
    => (Person (first-name="Oscar") (last-name="Diggs"))

TABLE

    let principals = Table of Person 
              "Dorothy" "Gale"
              "Oscar" "Diggs"

    var characters = Table of Person

    print principals
    => (Table of Person ("Dorothy" "Gale") ("Oscar" "Diggs"))

    print characters
    => (Table of Person)

    let new-list = append characters (Person "Oscar" "Diggs")
 
    alter characters append (Person "Oscar" "Diggs")


(Table of Person) is still Table-type. "of Person" is type’s "parameter"
"of" always insert type-parameters. eg::

    var amount = number of (14 2) 1,245,667.23
    var amount = number (14 2) 1,245,667.23
    var amount = number (scale=14) (precision=2) 1,245,667.23
    var amount = number (14) 125,234
    var amount = number (scale=12) 324322

    //automatic?
    alias Person-Table = Table of Person 

    var characters = Person-Table
                   "Dorothy" "Gale"
                   "Oscar" "Diggs"

    let main-characters = Person-Table 
              "Dorothy" "Gale"
              "Oscar" "Diggs"

ARRAY

    let names = Array of string "Dorothy" "Glinda" "Oscar"
    let names = string-Array "Dorothy" "Glinda" "Oscar"

    let names-list = List "Dorothy" "Glinda" "Oscar"

    var names-array = Array of string "Dorothy" "Glinda" "Oscar"

    var names-list = "Dorothy" "Glinda" "Oscar" 
    // ok because ("Dorothy" "Glinda" "Oscar")=>(list "Dorothy" "Glinda" "Oscar")


DICTIONARY

    let appeareances = Dictionary of Int 
               "Dorothy" 124
               "Glinda" 39
               "Oscar" 23

    var appeareances = Dictionary of Int 
               "Dorothy" 124
               "Glinda" 39
               "Oscar" 23


2.1 - the same goes for *Interface*

    let Openable = interface 

        abstract open
        abstract close
        abstract walk-in

        method enter-and-close (msg:string amount:decimal=1 -> string)
             open this
             walk-in this
             close this

        method enter-and-close ((param msg string) (param amount decimal 1) (returning string))

*method macro-expands to:*

    let Openable/enter-and-close = function (this:Openable  msg:string  amount:decimal -> string )
           open this
           walk-in this
           close this
           return "ok"


    let Door = struct 
         implements Openable

         field is-open boolean 

         method open
             set is-open true

         method close
             set is-open false

         method walk-in
             if (not is-open) (fail-with "the door is closed")
             print "passing thru"


    var my-door = Door (is-open=false)
    => bind my-door (variable-storage (Door (is-open=false)))

    var other-door // initializes to zero-value
    => bind other-door (variable-storage (Door))

    var a-int = null
    => bind a-int (variable-storage (Int null))
    =>ERROR: Cannot create (Int) from null

    var a-int = "1245"
    => bind a-int (variable-storage (Int null))
    =>ERROR: Cannot create (Int) from (string "1245")

    var a-number= null
    => bind a-number (variable-storage (Number null))
    =>OK: typer Number *can* be null

    log my-door //log show all the info
    => (variable-storage (Door (is-open=false)))

    print my-door //print skips variable-storage, and goes to the value
    => (Door (is-open=false))

    print a-number
    => (Number null)

    let z = my-door
    => bind z (constant-storage (type=Door) (value=(Door (is-open=false)))

    log z
    => (constant-storage (type=Door) (value=(Door (is-open=false)))
    
    print z
    => (Door z (is-open=false))

FUNCTION SIGNATURE
------------------

    OPC SIGNATURE 1, solo "=". tema: c=Person => c = (Person "" "") => todo tiene default

    let fnx = function ( this=Person name=string age=(Int 50)  -> string )   


    OPC SIGNATURE 2, ":" y "="
    let fnx = function ( this:Person name:string="hello" age:Int=10 -> string )   

    here only "age" is optional

calls:

    print (fnx (Person "pepe" "marrone") age=50 )
    
    print (fnx name="" age=40 this=(Person "pepe" "marrone"))

    print (fnx this=(Person "pepe" "marrone") age=60 )


  NOTE: ":" y "=" only valid for signatures.

     var z:string="aa" IS NOT VALID

     var z = string "aa" IS THE CORRECT FORM - consistent initializer


INTERFACES
----------
    let OpenDock-able = Interface
         include Openable Lockable

    var my-door = Something OpenDock-able d




*macro-expand to:*
    
    bind my-door (OpenDock-able (Door d (is-open=false)))

    print o
    => (OpenDock-able (Door d (is-open=false)))

    var o = d
    print o
    => (Door (is-open=false))

    var o1 (Something)
    print o1
    => (Interface-value Something null)

    set o1 = d
    print o1
    => (Interface-value (interface-type=Something) (concrete=(Door (is-open=false)))
