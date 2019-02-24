# Change some Arabic characters when querying in Oracle
## Firstly, you need to use replace function to replace أ to ا
 ```REPLACE(p_text, 'أ', 'ا' )```
 ### Replace another character like ة to ه
 ```REPLACE(REPLACE(p_text, 'ة', 'ه' ), 'أ', 'ا' )```
 ### And another character like ى to ي and آ to ا and إ to ا
 ```REPLACE(REPLACE(REPLACE(REPLACE(REPLACE(p_text, 'أ', 'ا' ), 'إ', 'ا' ), 'ه', 'ة' ), 'آ', 'ا'), 'ى', 'ي')```
 
 ## Lower the search key value and trim spaces 
  > lower: make all the letters of the English language in lower case
  
  > trim: remove spaces from start and end of the search key value.
 ```
     REPLACE(
        REPLACE(
           REPLACE(
              REPLACE(
                 REPLACE(
                    lower(
                      trim(p_text)
                          ),
                  'أ', 'ا' )
              , 'إ', 'ا' )
          , 'ه', 'ة' )
        , 'آ', 'ا')
     , 'ى', 'ي')
   ```
 ## Secondary, Make a function and pass key value.
  ```
    Create or Replace function character_replacement(p_text in varchar2) return varchar2 as 
    begin 
      return 
            REPLACE(
                REPLACE(
                   REPLACE(
                      REPLACE(
                         REPLACE(
                            lower(
                              trim(p_text)
                                  ),
                          'أ', 'ا' )
                      , 'إ', 'ا' )
                  , 'ه', 'ة' )
                , 'آ', 'ا')
             , 'ى', 'ي');
      end ;
 ```
 ## Finally, Use it in a query.
 ```select * from books where character_replacement(book_name) like character_replacement('كتاب البرمجة في قواعد البيانات')```
