# XppML - Immerse your X++ code in XML and make it machine understandable.

XppML is an add-on to Microsoft Dynamics AX2012 R3 which allows you to _XMLize_ your X++ source. 
# Why
Making your source code understandable opens it up to:
 - machine learning
 - automated code reviews

      *finding faulty code patterns beyond any standard BP checks*
 - automated code manipulation / rewrite(s).

      *e.g: automatically split RunBase classes into three classes adhering to MVC pattern.*

  ### We already have XREF and BP?
  The standard XREF AX tool is useful to locate places where a particular object has been referenced, but it adds nothing toward making the source code machine understandable nor does it allow any particular sub-context of how the code has been used / constructed. The vast majority of BP checks do not extend beyond the particular method being checked.

# How
## Step 1: From X++ to XppML
  
|Line| X++ |  XppML
|--|--|--|
|1 | public void modifiedField(FieldId _fieldId) |&lt;source&gt;&lt;preamble&gt;&lt;kw&gt;**public**&lt;/kw&gt;&lt;kw&gt;**void**&lt;/kw&gt;&lt;def&gt;&lt;method&gt;**modifiedField**&lt;/method&gt;**(**&lt;el&gt;&lt;decl&gt;&lt;type domain="edt"&gt;**FieldId**&lt;/type&gt;&lt;var domain="edt" type="FieldId"&gt;**_fieldId**&lt;/var&gt;&lt;/decl&gt;&lt;/el&gt;**)**&lt;/def&gt;&lt;/preamble&gt;
|2 | { |**{**&lt;bl&gt;
|3 | super(_fieldId); | &lt;st&gt;&lt;call&gt;&lt;method name="modifiedField" table="Common"&gt;&lt;kw&gt;**super**&lt;/kw&gt;&lt;/method&gt;**(**&lt;el&gt;&lt;var domain="edt" type="FieldId"&gt;**_fieldId**&lt;/var&gt;&lt;/el&gt;**)**&lt;/call&gt;&lt;/st&gt;**;**
|4 | } | &lt;/bl&gt;**}**&lt;/source&gt;

## Step 2: Save in the database
The resulting xml documents are saved into an xml-aware database

## Step 3: Use XPath/XQuery and get deep insights into your code

| Query | XPath |
|--|--|
|Find if statements without {} | //st[kw="if" and not(bl)] | statements containing keyword 'if' and not containing the block
|Find methods returning void having only one parameter |/source/preamble[kw[.="void"] and count(def/el)=1]
|When supplied, what is the 3rd parameter in all error(...) calls? | //call[method[.="error" and @domain="class" and @type="Global"]]/el[3]| 
