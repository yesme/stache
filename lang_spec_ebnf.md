{stache} Language Spec in ANTLR
===============================

    FLOAT_LITERAL = '-'? ('0'..'9')+ '.' ('0'..'9')* EXPONENT?
        |  '-'? '.' ('0'..'9')+ EXPONENT?
        |  '-'? ('0'..'9')+ EXPONENT
        ;

    EXPONENT = ('e'|'E') ('+'|'-')? ('0'..'9')+ ;
    
    BOOL_LITERAL = 'true' | 'false' ;
