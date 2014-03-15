Compatible with Java1.5+


Field Level Security Framework helps to move the expressions away from UI/Presentation/Business layer to a file system or a DB table. Follows the Separation of Concerns. 
Field Level Security Framework is flexible for the both Token / Role based systems.

The biggest advantage of this Framework is, we can change the Rule or Expressions at runtime without modifying the Application. Because the Expressions are in the the database. Just reload the Expressions only once.

Framework provides the Flexibility of loading data from a File / DB / Service (must implement IRuleService)
Either let the Framework read the data from DB on its own or create services to read data from DB and plugin the services to the Framework.

Framework is optimezed for Performance.
All the Rules and the Expressions processed only once (while loading the data) and reused at runtime by just replacing the properties.
A Rule is a combination of multiple Expressions (logically divided)
An Expression can be a simple or complex. (I personally prefer simple, becoz a Rule is a combination of Expressions, beauty of this framework)
Expressions will be written using Java Script Expressions
Expressions can be a Static or Dynamic
A Static Expression can be a Simple true / false or a property of the class can be verified against a static/literal/constant value
A Dynamic Expression is used to compare the values of Two or mow objects.

Framework uses Java Script Engine to evaluate the Expression.

Currently, the Framework has been coded to satisfy most of the scenarios
Compares a Property with 
1) String, Number, Date, Property of the Object -- Static
2) List<String> ([String]), List<Number> ([Number]), List<Date> ([Date]), List<UserDefinedObject> ([property]) -- Dynamic


For Static:
sf.rendered('EditBtn', tokens);
sf.disabled('EditBtn', tokens);
sf.rendered('EditBtn', tokens, obj);
sf.disabled('EditBtn', tokens, obj);
sf.rendered('EditBtn', tokens, new Object[]{obj1, obj2, ...});
sf.disabled('EditBtn', tokens, new Object[]{obj1, obj2, ...});

For Dynamic:
sf.rendered('EditBtn', tokens, obj, dynamicObject);
sf.disabled('EditBtn', tokens, obj, dynamicObject);
sf.rendered('EditBtn', tokens, obj, new Object[]{dynamicObject1, dynamicObject2, ...});
sf.disabled('EditBtn', tokens, obj, new Object[]{dynamicObject1, dynamicObject2, ...});
sf.rendered('EditBtn', tokens, new Object[]{obj1, obj2, ...}, new Object[]{dynamicObject1, dynamicObject2, ...});
sf.disabled('EditBtn', tokens, new Object[]{obj1, obj2, ...}, new Object[]{dynamicObject1, dynamicObject2, ...});
sf.rendered('EditBtn', tokens, new Object[]{obj1, obj2, ...}, dynamicObject);
sf.disabled('EditBtn', tokens, new Object[]{obj1, obj2, ...}, dynamicObject);

Advantages:
Rule or Expression can be updated at any point of time without deployin the Application.
Expressions can be reloaded.
Enhanced performace for faster processing(intentionally, most of the code has been duplicated for better performance).
Can be used as a Singleton Class or a new instance for each session for Performance improvments.
Can be used as a standalone by declaring the rules in the File.
Can be used in a Web or Enterprise Applications to validate the same rules (No need to Duplicate the Expressions or the Logic to validate the data).
Enhanced Exception handling (Provides notification of each and every issue)
Lightweight jar, just add it to the classpath and use it.
Works with any technology/framework (JSF, Spring, ...)
Works in any layer (UI / Presentation / Business)


Limitations:
Application has to be updated when an Rule/Expression has been updated/introduced for a new property which is from a New/Another Object
Won't processes the Complex Object (property.property.property. ...., but works with the Inheritence). Alternative is, you can pass the final object which contains the property.


=================================File: SecurityData.txt=================================

##Note: Column Order is important
#Note: Do not worry about the Column/Table names; Follow the Column Order
#Token(token name, token desc)
#Expression(exp name, exp desc, exp type, property, value expression)
#Rule(rule name/code, rule desc, token name, expression)

#sample tables
#token(token_c varchar2, token_x varchar2)
#rule(rule_c varchar2, rule_x varchar2)
#expression(expression_c varchar2, expression_x varchar2, expression_type_c char(1) (allowed S/D), property_x varchar2, value_expr_x varchar2
#secrutity_permission(rule_c, token_c, rule_expr_x varchar2 (combination of expression_c)


Token:sec.admin~Team - Admin
#Token:sec.update~Team - Update
#Token:sec.ps~Property Services Team
#Token:sec.t1~T1 Team

#Expression:E1~Expression for TRUE~S~TRUE~true
#Expression:E2~Expression for FALSE~S~FALSE~false
#Expression:E3~Expression For status_C~S~status_C~?: == 'ACTIVE' || ?: == 'OPEN' || ?: == 'ASDF'
#Expression:E4~Expression For status_C~S~status_C~['ABANDON','FILLED'].indexOf(?:) != -1
#Expression:E5~Expression for bridgeManager~S~bridge_type_c~?: == 'R'
#Expression:E6~Dynamic Expression Test~D~bridgeManager~?: == userId || ?: == userId || ?: == userId
#Expression:E7~Dynamic Expression Test~D~experience~?: >= expMin
#Expression:E8~Dynamic Expression Test~D~experience~?: >= expMin && ?: <= expMax
#Expression:E9~Date Test~D~dateOfBirth~?: == anotherDate
#Expression:E10~Date Test~D~dateOfBirth~?: >= anotherDate
#Expression:E11~Date Test~D~dateOfBirth~?: >= anotherDate && ?: <= anotherDate_1
Expression:E12~Dynamic Expression Test [String]~D~bridgeManager~( ( [String].indexOf ( ?: ) != -1  ) || (  ?: == 'Ganesh'  ) )   
#Expression:E13~Dynamic Expression Test [Number]~D~experience~[Number].indexOf(?:) != -1
#Expression:E14~Dynamic Expression Test [Date]~D~anotherDate~[Date].indexOf(?:) != -1
Expression:E15~Dynamic Expression Test [anotherProperty]~D~bridgeManager~([userId].indexOf(?:)!=-1)


#Rule:DateRuleTest1~Date Rule Test1~sec.admin~E9
#Rule:DateRuleTest2~Date Rule Test2~sec.admin~E10
#Rule:DateRuleTest3~Date Rule Test3~sec.admin~E11
#Rule:DynamicRule~Rule for Dynamic Value~sec.admin~E6
#Rule:DynamicComplexRule~Rule for Dynamic Value with Complex expressions~sec.admin~(E3 || E4) && E5 && E6 && (E7 || E8)
#Rule:ComplexRule~Rule for Multiple Objects~sec.admin~(E4 && E5)
#Rule:RenderAddBtn~Rule For Add Btn For Documents~sec.admin~E1
#Rule:DisableAddBtn~Rule For Add Btn For Documents~sec.admin~E2
#Rule:AddBtn~Rule For Add Btn For Documents~sec.update~E1
#Rule:UpdtBtn~Rule for Update or Save Btn for Bridge~sec.admin~(E3 || E4)
#Rule:UpdtBtn~Rule for Update or Save Btn for Bridge~sec.ps~E4
#Rule:UpdtBtn~Rule for Update or Save Btn for Bridge~sec.t1~(E4 && E5)
#Rule:UpdtBtn~Rule for Update or Save Btn for Bridge~sec.update~((E3 || E4) && E5)
#Rule:TESTRule1~asdf~sec.admin~E3 && E6
Rule:TESTRule2~asdf~sec.admin~E15

==========================File============================================

======================Database=============================================

dataAccessMode=Query
Token=select token_c, token_x from token
Expression=select expression_c, expression_x, expression_type_c, property_x, value_expr_x from expression
Rule=select sr.rule_c, sr.rule_x, st.token_c, SP.RULE_EXPR_X from permission sp join rule sr on SP.RULE_C = SR.RULE_C join token st on SP.TOKEN_C = ST.TOKEN_C

CREATE TABLE token
(
  TOKEN_C              VARCHAR2(100 BYTE) primary key,
  TOKEN_X              VARCHAR2(150 BYTE),
);


CREATE TABLE expression
(
  EXPRESSION_C         VARCHAR2(50 BYTE) primary key,
  EXPRESSION_X         VARCHAR2(150 BYTE),
  EXPRESSION_type_c    CHAR(1 BYTE)             NOT NULL,
  PROPERTY_X           VARCHAR2(50 BYTE),
  VALUE_EXPR_X         VARCHAR2(500 BYTE),
);

CREATE TABLE rule
(
  RULE_C               VARCHAR2(100 BYTE) primary key,
  RULE_X               VARCHAR2(150 BYTE),
);

CREATE TABLE permission
(
  RULE_C               VARCHAR2(100 BYTE)       NOT NULL,
  TOKEN_C              VARCHAR2(100 BYTE)       NOT NULL,
  RULE_EXPR_X          VARCHAR2(1000 BYTE)      NOT NULL,
  constraint sp_pk primary key(rule_c, token_C)

);

--Tokens:
insert into token values ('security.admin',' Team - Admin');
insert into token values ('security.update',' Team - Update');

--Expressions:
insert into expression values('E1','Expression for TRUE','S','TRUE','true');
insert into expression values('E2','Expression for FALSE','S','FALSE','false');
insert into expression values('E3','Expression to Evaluate Capacity Rating value','S','type_c','?: == ''R''');

--Rule
insert into rule values ('showAdminMenu','Render Admin Menu');
insert into rule values ('showDocDelBtnOn','Render Document Delete Button on ');
insert into rule values ('showAddPhotoBtn','Render Add Photo Button');


--Permissions:


-- Team - Admin:
insert into permission values ('showAdminMenu','security.admin','E1');
insert into permission values ('showAddPhotoBtn','security.admin','E1');
insert into permission values ('showAddDocBtn','security.admin','E1');



    
commit;

======================Database=============================================
