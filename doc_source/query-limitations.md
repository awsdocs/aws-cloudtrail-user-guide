# CloudTrail Lake SQL constraints<a name="query-limitations"></a>

CloudTrail Lake queries are SQL strings\. This section describes the allowed SQL language that you use to create queries\.

Only `SELECT` statements are allowed\. No query strings can change or mutate data\. The API restricts the scope of a `SELECT` statement to the argument tree shown in the following template\. Simple aggregations, conditions, and operators are allowed\. A keyword, operator, or function that is not described in this section is disallowed\. The event data store ID—the ID portion of the event data store's ARN—is the valid value for tables\.

```
SELECT [ DISTINCT ] columns [ Aggregate ]
[ FROM Tables event_data_store_ID]
[ WHERE columns [ Conditions ] ]
[ GROUP BY columns [ DISTINCT | Aggregate ] ]
[ HAVING columns [ Aggregate | Conditions ] ]
[ ORDER BY columns [ Aggregate | ASC | DESC | NULLS | FIRST | LAST ]
[ LIMIT [ INT ] ]
```

**Topics**
+ [Supported schema for event record ﬁelds](#query-supported-event-schema)
+ [Aggregate functions and condition operators](#query-aggregates-condition-operators)
+ [Supported functions](#query-supported-functions)

## Supported schema for event record ﬁelds<a name="query-supported-event-schema"></a>

The following is the valid SQL schema for event record fields\.

```
[
    {
        "Name": "eventversion",
        "Type": "string"
    },
    {
        "Name": "useridentity",
        "Type": "struct<type:string,principalid:string,arn:string,accountid:string,accesskeyid:string,
                 username:string,sessioncontext:struct<attributes:struct<creationdate:timestamp,
                 mfaauthenticated:string>,sessionissuer:struct<type:string,principalid:string,arn:string,
                 accountid:string,username:string>,webidfederationdata:struct<federatedprovider:string,
                 attributes:map<string,string>>,sourceidentity:string,ec2roledelivery:string,
                 ec2issuedinvpc:string>,invokedby:string,identityprovider:string>"
    },
    {
        "Name": "eventtime",
        "Type": "timestamp"
    },
    {
        "Name": "eventsource",
        "Type": "string"
    },
    {
        "Name": "eventname",
        "Type": "string"
    },
    {
        "Name": "awsregion",
        "Type": "string"
    },
    {
        "Name": "sourceipaddress",
        "Type": "string"
    },
    {
        "Name": "useragent",
        "Type": "string"
    },
    {
        "Name": "errorcode",
        "Type": "string"
    },
    {
        "Name": "errormessage",
        "Type": "string"
    },
    {
        "Name": "requestparameters",
        "Type": "map<string,string>"
    },
    {
        "Name": "responseelements",
        "Type": "map<string,string>"
    },
    {
        "Name": "additionaleventdata",
        "Type": "map<string,string>"
    },
    {
        "Name": "requestid",
        "Type": "string"
    },
    {
        "Name": "eventid",
        "Type": "string"
    },
    {
        "Name": "readonly",
        "Type": "boolean"
    },
    {
        "Name": "resources",
        "Type": "array<struct<accountid:string,type:string,arn:string,arnprefix:string>>"
    },
    {
        "Name": "eventtype",
        "Type": "string"
    },
    {
        "Name": "apiversion",
        "Type": "string"
    },
    {
        "Name": "managementevent",
        "Type": "boolean"
    },
    {
        "Name": "recipientaccountid",
        "Type": "string"
    },
    {
        "Name": "sharedeventid",
        "Type": "string"
    },
    {
        "Name": "annotation",
        "Type": "string"
    },
    {
        "Name": "vpcendpointid",
        "Type": "string"
    },
    {
        "Name": "serviceeventdetails",
        "Type": "map<string,string>"
    },
    {
        "Name": "addendum",
        "Type": "map<string,string>"
    },
    {
        "Name": "edgedevicedetails",
        "Type": "map<string,string>"
    },
    {
        "Name": "insightdetails",
        "Type": "map<string,string>"
    },
    {
        "Name": "eventcategory",
        "Type": "string"
    },
    {
        "Name": "tlsdetails",
        "Type": "struct<tlsversion:string,ciphersuite:string,clientprovidedhostheader:string>"
    },
    {
        "Name": "sessioncredentialfromconsole",
        "Type": "string"
    },
    {
        "Name": "eventjson",
        "Type": "string"
    }
    {
        "Name": "eventjsonchecksum",
        "Type": "string"
    }
]
```

## Aggregate functions and condition operators<a name="query-aggregates-condition-operators"></a>

The following are allowed `Aggregate` functions\.

```
SUM
MIN
MAX
AVG
COUNT
```

The following are allowed `Condition` operators\.

```
AND
OR
IN
NOT
IS (NOT) NULL
LIKE
<
>
<=
>=
<>
!=
( conditions ) #parenthesised conditions
```

## Supported functions<a name="query-supported-functions"></a>

The following are supported functions for CloudTrail Lake queries\. For descriptions and examples, see [JSON Functions and Operators](https://prestodb.io/docs/current/functions/json.html) on the Presto 0\.266 documentation website\.


| Function | 
| --- | 
| element\_at\(Map \| Array, Object \| number\) ➝ Object | 
| cardinality\(Map \| Array\) ➝ BigInt | 
| date convert functions \(see following table\) | 
| map\_values\(Map\) ➝ Array\(Object\) | 
| map\_keys\(Map\) ➝ Array\(Object\) | 
| contains\(array, object\) ➝ boolean | 
| array\_distinct\(array\) ➝ array | 
| array\_max\(array\) ➝ Object, array\_min\(array\)➝ Object | 
| slice\(array, start, length\) ➝ array | 
| json\_parse\(string\) ➝ json | 
| is\_json\_scalar\(json\) ➝ boolean | 
| json\_extract\(json, string\) ➝ Json | 
| json\_extract\_scalar\(json, string\) ➝ string | 
| json\_format\(json\) ➝ string | 
| json\_array\_contains\(json\_array, object\) ➝ boolean | 
| json\_array\_get\(json\_array, index\) ➝ json | 
| json\_array\_length\(json\_array\)➝ bigInt | 

### Supported date convert functions<a name="query-supported-date-convert-functions"></a>

For more information about the supported date and time functions, see [Date and Time Functions and Operators](https://prestodb.io/docs/current/functions/datetime.html) on the Presto 0\.266 documentation website\.


| Date convert functions | 
| --- | 
|  `date`  | 
|  `date_trunc`  | 
|  `day`  | 
|  `day_of_month`  | 
|  `day_of_week`  | 
|  `day_of_year`  | 
|  `dow`  | 
|  `doy`  | 
|  `hour`  | 
|  `millisecond`  | 
|  `minute`  | 
|  `month`  | 
|  `quarter`  | 
|  `second`  | 
|  `week`  | 
|  `week_of_year`  | 
|  `year`  | 
|  `year_of_week`  | 
|  `yow`  | 