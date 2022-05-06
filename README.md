# PL-SQL
tutorial

## use Array (Select multidata)  
for array can use  VARRAY datatype in PS/SQL    

```sql
create or replace PROCEDURE UPDATE_FIELD_RULE_LIST

-- input arguments here ...

       IS

         field_rule_id NUMBER;

         --  create new type array of data
         TYPE ARRYA_DATA IS VARRAY(10) OF TBL_FIELD_RULE%ROWTYPE ;
         -- define new variable with created data type for selected data
         selected_rule ARRYA_DATA;

         -- define another variable for input data
         rule_id_list ARRYA_DATA;

       BEGIN

        -- add empty data to array after begin (have to do it)
        rule_id_list := ARRYA_DATA();
        -- extend array count not more than defined array length(here max length = 10)
        rule_id_list.extend(10);

       SELECT  
           PRODUCT_ID ,
           FIELD_ID ,
           RULE_ID ,
           IS_ACTIVE  
       BULK COLLECT INTO
           -- save selected value inside array
           selected_rule
       FROM
           TBL_FIELD_RULE
       WHERE
           PRODUCT_ID = 1
           AND FIELD_ID = 6;

        FOR indx IN 1..selected_rule.COUNT LOOP
            -- test save values from select query
            dbms_output.put_line(selected_rule(indx).RULE_ID);
            -- store above value inside new array
            rule_id_list(indx) := selected_rule(indx);
        END LOOP;

--        COMMIT;
       END;
```

 Use table format instead of array to store selected multi data.

```sql
create or replace PROCEDURE UPDATE_FIELD_RULE_LIST

-- input arguments here ...

       IS

         selected_rule TBL_FIELD_RULE%ROWTYPE;
         TYPE FIELD_RULE_LIST_TYPE IS TABLE OF TBL_FIELD_RULE%ROWTYPE;
         selected_rule_list FIELD_RULE_LIST_TYPE;

       BEGIN

--     Select Rule List Of Filed
       SELECT  
           PRODUCT_ID ,
           FIELD_ID ,
           RULE_ID ,
           IS_ACTIVE  
       BULK COLLECT INTO
           selected_rule_list
       FROM
           TBL_FIELD_RULE
       WHERE
           PRODUCT_ID = 1
           AND FIELD_ID = 6;

        FOR indx IN 1..selected_rule_list.COUNT LOOP
            dbms_output.put_line(selected_rule_list(indx).RULE_ID);
        END LOOP;

--        COMMIT;
       END;

```
