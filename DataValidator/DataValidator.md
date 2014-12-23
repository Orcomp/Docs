[TOC]

DataValidator VXML schema
=========================

Basic validation rules are defined in a XML file which looks like:

```
<DataValidator xmlns="http://orcomp/2014">
    <CsvValidator EntityName="Orders" Path="Pop Orders.csv">
        <Field Name="Ord_No">
            <IsContained In="OrderDetails.Ord_No" />
        </Field>
        <Field Name="Due_Dt" Type="DateTime">
            <GreaterThan FieldName="Last MRP Regen Date"/>
        </Field>
    </CsvValidator>
</DataValidator>
```

VXML structure
-----------------------

- DataValidator - is the root element
- CsvValidator - represents a file (or "entity") to validate.   
    - Attributes:
        - EntityName - entity name. This is the name which will be used to identify the csv file. It is mainly used in the "IsContained" rule.
        - Path - The path can either be a relative path to a CSV file with respect to the VXML directory, or an absolute path "C:\My Folder\Projects\ABC\Xyz.csv"

**Note:** Network paths are also accepted.

- Field - represents an entity field to validate.
    - Attributes:
        - Name - name of the field to validate. A column name in the majority of cases.
        - Type - type of the field. Each field value will be validated to match to the given type. Default value for this attribute is "string". Valid types are:
              - String
              - Integer
              - Decimal
              - DateTime
              - TimeSpan

**Note:** Each field can contain multiple nested validation rules.

List of validation rules
==================

```
<AllowedValues Values="YES,NO,N/A" ThrowOnError="True"/>
```
Each validation rule can have a ThrowOnError attribute added to it. When ThrowOnError=True, then validation will be stopped after the first failed validation, and an exception will be thrown.

Condition Validation Rules
-----------------------------------

```
<LessThan FieldName="Last MRP Regen Date"/>
<LessThan Value="2/12/2014 12:00"/>
<LessOrEqualTo FieldName="Last MRP Regen Date"/>
<EqualTo FieldName="Last MRP Regen Date"/>
<GreaterThan FieldName="Last MRP Regen Date"/>
<GreaterOrEqualTo Value="3/11/2014 12:00"/>
```
Validates the field to be greater/less/equal to a constant value or another field of the same entity.

Attributes:

- FieldName: name of the field to compare with
- Value: a constant value to compare with

Note: Either "FieldName" or "Value" can be used, but not both at the same time.

Other validation rules
----------------------------

### AllowedValues
```
<AllowedValues Values="YES,NO,N/A" />
```
A comma-separated list of allowed values.

### IsContained

```
<IsContained In="OrderDetails.Ord_No" />
```
Validates that each field value is found in a column in another csv file (defined by its EntityName.)

- Attributes:
    - In: path to the field to search the value in

### NotNull
```
<NotNull />
```
Validates that the value is not empty or has only whitespaces.

### RegEx

[RegEx or Regular expressions](http://en.wikipedia.org/wiki/Regular_expression) is a powerful pattern matching syntax.

```
<RegEx Match="\d+"/>
```

Validates that the value matches to the given RegEx pattern.

**Note:** RegEx is an advanced feature that requires advanced skills. [This website](http://jex.im/regulex/) can assist you in crafting good expression.


### StringLength
```
<StringLength MinLength="20" MaxLength="60"/>
```
Validates that the given value has a defined number of characters.

- Attributes:
    - MinLength - min allowed length
    - MaxLength - max allowed length
    - ExactLength - exact length

    MinLength and MaxLength cannot be used with ExactLength at the same time.

### Unique
```
<Unique />
```
Validates that each value is unique within the column values.

```
<Field Name="GroupID">
    <Unique CombineWith="FromSubGroupID,PartID"/>
</Field>
```
Unique validation rule also allows to validate field combinations for uniqueness.
The example shows how to validate that the combination of GroupID, FromSubGroupID and PartID is uniques for each row.


XML
=====

Comments
--------------

To comment a line or a block of lines in XML you can use this:

````
<!-- Comment this line -->
````