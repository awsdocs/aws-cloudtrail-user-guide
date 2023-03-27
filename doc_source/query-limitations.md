# CloudTrail Lake SQL constraints<a name="query-limitations"></a>

CloudTrail Lake queries are SQL strings\. This section describes the allowed SQL language that you use to create queries\.

Only `SELECT` statements are allowed\. No query strings can change or mutate data\. The API restricts the scope of a `SELECT` statement to the argument tree shown in the following template\. A keyword, operator, or function that is not described in this section is disallowed\. The event data store ID—the ID portion of the event data store's ARN—is the valid value for tables\.

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
+ [Supported schema for CloudTrail event record ﬁelds](#query-supported-event-schema)
+ [Supported schema for AWS Config configuration item record ﬁelds](#query-supported-config-items-schema)
+ [Supported schema for AWS Audit Manager evidence record ﬁelds](#query-supported-event-schema-audit-manager)
+ [Supported schema for non\-AWS event ﬁelds](#query-supported-event-schema-integration)
+ [Aggregate functions, condition and join operators](#query-aggregates-condition-operators)
+ [Supported functions](#query-supported-functions)
+ [Advanced, multi\-table query support](#query-advanced-multi-table)

## Supported schema for CloudTrail event record ﬁelds<a name="query-supported-event-schema"></a>

The following is the valid SQL schema for CloudTrail event record fields\.

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

## Supported schema for AWS Config configuration item record ﬁelds<a name="query-supported-config-items-schema"></a>

The following is the valid SQL schema for configuration item record fields\. For configuration items, the value of `eventcategory` is `ConfigurationItem`, and the value of `eventtype` is `AwsConfigurationItem`\.

```
[
    {
        "Name": "eventversion",
        "Type": "string"
    },
    {
        "Name": "eventcategory",
        "Type": "string"
    },
    {
        "Name": "eventtype",
        "Type": "string"
    },
        "Name": "eventid",
        "Type": "string"
    },
    {
        "Name": "eventtime",
        "Type": "timestamp"
    },
    {
        "Name": "awsregion",
        "Type": "string"
    },
    {
        "Name": "recipientaccountid",
        "Type": "string"
    },
    {
        "Name": "addendum",
        "Type": "map<string,string>"
    },
    {
        "Name": "eventdata",
        "Type": "struct<configurationitemversion:string,configurationitemcapturetime:
                 string,configurationitemstatus:string,configurationitemstateid:string,accountid:string,
                 resourcetype:string,resourceid:string,resourcename:string,arn:string,awsregion:string,
                 availabilityzone:string,resourcecreationtime:string,configuration:map<string,string>,
                 supplementaryconfiguration:map<string,string>,relatedevents:string,
                 relationships:struct<name:string,resourcetype:string,resourceid:string,
                 resourcename:string>,tags:map<string,string>>"
    }
]
```

## Supported schema for AWS Audit Manager evidence record ﬁelds<a name="query-supported-event-schema-audit-manager"></a>

The following is the valid SQL schema for Audit Manager evidence record fields\. For Audit Manager evidence record fields, the value of `eventcategory` is `Evidence`, and the value of `eventtype` is `AwsAuditManagerEvidence`\. For more information about aggregating evidence in CloudTrail Lake using Audit Manager, see [Evidence finder](https://docs.aws.amazon.com/audit-manager/latest/userguide/evidence-finder.html) in the *AWS Audit Manager User Guide*\.

```
[
    {
        "Name": "eventversion",
        "Type": "string"
    },
    {
        "Name": "eventcategory",
        "Type": "string"
    },
    {
        "Name": "eventtype",
        "Type": "string"
    },
        "Name": "eventid",
        "Type": "string"
    },
    {
        "Name": "eventtime",
        "Type": "timestamp"
    },
    {
        "Name": "awsregion",
        "Type": "string"
    },
    {
        "Name": "recipientaccountid",
        "Type": "string"
    },
    {
        "Name": "addendum",
        "Type": "map<string,string>"
    },
    {
        "Name": "eventdata",
        "Type": "struct<attributes:map<string,string>,awsaccountid:string,awsorganization:string,
                 compliancecheck:string,datasource:string,eventname:string,eventsource:string,
                 evidenceawsaccountid:string,evidencebytype:string,iamid:string,evidenceid:string,
                 time:timestamp,assessmentid:string,controlsetid:string,controlid:string,
                 controlname:string,controldomainname:string,frameworkname:string,frameworkid:string,
                 service:string,servicecategory:string,resourcearn:string,resourcetype:string,
                 evidencefolderid:string,description:string,manualevidences3resourcepath:string,
                 evidencefoldername:string,resourcecompliancecheck:string>"
    }
]
```

## Supported schema for non\-AWS event ﬁelds<a name="query-supported-event-schema-integration"></a>

**Note**  
Currently, you can log activity events from non\-AWS events in all commercial AWS Regions supported by CloudTrail Lake except: me\-central\-1\. For information about CloudTrail Lake supported Regions, see [CloudTrail Lake supported Regions](cloudtrail-lake-supported-regions.md)\. 

The following is the valid SQL schema for non\-AWS events\. For non\-AWS events, the value of `eventcategory` is `ActivityAuditLog`, and the value of `eventtype` is `ActivityLog`\.

```
[
    {
        "Name": "eventversion",
        "Type": "string"
    },
    {
        "Name": "eventcategory",
        "Type": "string"
    },
    {
        "Name": "eventtype",
        "Type": "string"
    },
        "Name": "eventid",
        "Type": "string"
    },
    {
        "Name": "eventtime",
        "Type": "timestamp"
    },
    {
        "Name": "awsregion",
        "Type": "string"
    },
    {
        "Name": "recipientaccountid",
        "Type": "string"
    },
    {
        "Name": "addendum",
        "Type": "struct<reason:string,updatedfields:string,originalUID:string,originaleventid:string>"
    },
    {
        "Name": "metadata",
        "Type": "struct<ingestiontime:string,channelarn:string>"
    },
    {
        "Name": "eventdata",
        "Type": "struct<version:string,useridentity:struct<type:string,
                 principalid:string,details:map<string,string>>,useragent:string,eventsource:string,
                 eventname:string,eventtime:string,uid:string,requestparameters:map<string,string>>,
                 responseelements":map<string,string>>,errorcode:string,errormssage:string,sourceipaddress:string,
                 recipientaccountid:string,additionaleventdata":map<string,string>>"
    }
]
```

## Aggregate functions, condition and join operators<a name="query-aggregates-condition-operators"></a>

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

The following are allowed `JOIN` operators\. For more information about running multi\-table queries, see [Advanced, multi\-table query support](#query-advanced-multi-table)\.

```
UNION 
UNION ALL 
EXCEPT 
INTERSECT 
LEFT JOIN 
RIGHT JOIN 
INNER JOIN
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
|  `current_timestamp`  | 
|  `date`  | 
|  `date_add`  | 
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
|  `now`  | 
|  `quarter`  | 
|  `second`  | 
|  `week`  | 
|  `week_of_year`  | 
|  `year`  | 
|  `year_of_week`  | 
|  `yow`  | 

## Advanced, multi\-table query support<a name="query-advanced-multi-table"></a>

CloudTrail Lake supports advanced query language across multiple event data stores\. Only queries that do not include sub\-queries are supported\.
+ [`UNION|UNION ALL|EXCEPT|INTERSECT`](#query-multi-table-union)
+ [`LEFT|RIGHT|INNER JOIN`](#query-multi-table-left-right)

To run your query, use the start\-query command in the AWS CLI\. The following is an example, using one of the sample queries in this section\.

```
aws cloudtrail start-query
--query-statement "Select eventId, eventName from EXAMPLEf852-4e8f-8bd1-bcf6cEXAMPLE UNION Select eventId, eventName from EXAMPLEg741-6y1x-9p3v-bnh6iEXAMPLE UNION ALL Select eventId, eventName from EXAMPLEb529-4e8f9l3d-6m2z-lkp5sEXAMPLE ORDER BY eventId LIMIT 10;"
```

The response is a `QueryId` string\. To get the status of a query, run `describe-query`, using the `QueryId` value returned by `start-query`\. If the query is successful, you can run `get-query-results` to get results\.

### `UNION|UNION ALL|EXCEPT|INTERSECT`<a name="query-multi-table-union"></a>

This release adds *multi\-table* queries, or queries that you can run across multiple event data stores\. The following is an example query that uses `UNION` and `UNION ALL` to find events by their event ID and event name in three event data stores, EDS1, EDS2, and EDS3\. The results are selected from each event data store first, then results are concatenated, ordered by event ID, and limited to ten events\.

```
Select eventId, eventName from EDS1
UNION
Select eventId, eventName from EDS2
UNION ALL
Select eventId, eventName from EDS3 
ORDER BY eventId LIMIT 10;
```

### `LEFT|RIGHT|INNER JOIN`<a name="query-multi-table-left-right"></a>

This release adds *multi\-table* queries, or queries that you can run across multiple event data stores\. The following is an example query that uses `LEFT JOIN` to find all events from an event data store named `eds2`, mapped to `edsB`, that match those in a primary \(left\) event data store, `edsA`\. The returned events occur on or before January 1, 2020, and only the event names are returned\.

```
SELECT edsA.eventName, edsB.eventName, element_at(edsA.map, 'test')
FROM eds1 as edsA 
LEFT JOIN eds2 as edsB
ON edsA.eventId = edsB.eventId 
WHERE edsA.eventtime <= '2020-01-01'
ORDER BY edsB.eventName;
```