## Overview for Aligning Caliper and xAPI 
### Background for the Proposed "xCaliper" Solution
#### Premise
* clearly no benefit to having 2 overlapping edu focused, RDF triple event structured, data collection APIs out there
* xAPI, because of its flexibility and non-standard conforming approach to collect different, endpoint agreed upon, custom streams of data, and the availability in the market prior to Caliper, has gotten some early adoption traction in the edu space.
* A majority of xAPI usage focuses on non-standard, custom end-point agreed upon exchanges of event streams consisting of more basic event data streams.
* xAPI "profiles" or "recipes" are fairly new and do not cover the entire learning activity spectrum in detail or in an extensible,  semantically modeled form, but are a useful mechanism or aggregating a collection of data elements for a more well defined and consistent target scenario such as Assessment, Video etc.
* Caliper's design premise and implementation is to build a standardized event based data model. This model is optimized fo collecting deep / granular learning activity interactions with extensibility and a common education focused vocabulary and ontology. With that, the Caliper model covers, in detail, learning activity and related context via a consistent, standardized form via the baseline event structure and a related set of "metric profiles" representing individual learning activities and all related context.  The primary goal being that all learning activities sourced from a variety of providers can adhere to a common standard for representing and sharing all of their detailed learning activity data.
* the Caliper event model can also support, at the most basic level, much like xAPI, a very basic event encapsulation of more non-standard, customized endpoint agreed upon data streams as required.  However, although flexible and useful, this does not foster the interoperability of both basic and more deep learning activity data that Caliper is primarliy focused on in its well defined and structured event model.

#### xCaliper Assumptions and Rationale

* the xAPI statement level API and event model is primarily intended and used to date in order to package up and transmit a wide spectrum of customized, non-standardized data streams between a source collection/provider endpoint and a target storage/consumer endpoint.  This fosters the much used flexiblity at the expense of standardizing the stream structure and data streams per activities and other educational context entities and workflows. xAPI Recipes have recently been introduced just to begin to define some common and more esoteric "profiles" of data streams that in aggregate have a more well-defined structure and composition for certain activity based scenarios.

* Recipe use and true standardization is minimal to date with the large majority of xAPI usage applied to more basic, shallow and customized data collection between endpoints.

* Caliper's ability to support both a basic, non-standard custom data stream as well as its metric profile based more well defined and extensive learning activity event model, makes it highly suitable to be a super set to also represent both xAPI statement level API structured data streams as well as its predefined "recipe" based aggregates.  It would be impossible to consider the inverse of xAPI as a superset as the Caliper API and underlying event model has a more well defined, semantically rich, common vocabulary/ontology representation that would be difficult to fold into xAPI without substantial loss of fidelity.

* Caliper's resulting JSON representation can be a single standardied stream representation to be consumed by any endpoint regardless of whether the xAPI or Caliper APIs were utilied for colletion and transmission. This does required xAPI and Caliper supporting consumption endpoints to preferably support the single xCaliper unified JSON representation (as long as the xCaliper service is enabled if both xAPI and Caliper collection endpoints are in play), or worst case continue to support the xAPI standalone JSON as well as adding support for the  xCaliper unified JSON stream.

* Since the majority of xAPI usage is for non-standard, custom, and more shallow data exchanges between agreed upon endpoints, with little / no standardization across same learning activity types and contexts, this supports via this proposal for xCaliper to have Caliper act as the superset API and representation of the collection data streams.

* However, with that, those applications that are xAPI enabled, and whose requirements are satisified by a more basic data exchange, can continue to utliize the xAPI collection endpoint API to package and transmit events.  The xCaliper service "proxy" will be enabled to tranduce the xAPI collection API emitted raw stream to Caliper superset JSON prior to the target / storage endpoint receiving the stream in Caliper JSON form.

* As applications requirements broaden with the need for more standardized, consistent, deep learning analytics data across multiple learning activities regardless of where these are sourced, the Caliper API can be used fr data collection to support these ore detailed use cases

* the xAPI initial set of recipes, can be easiliy mapped and migrated to Caliper metric profiles and then both the extensive IMS Caliper community and the xAPI community can continue to collectively work on expanding metric profiles for cover more and more learning activity use case scenarios.

#### What is xCaliper ?

What is labeled and described in more detail throughout this proposed, is "xCaliper".  xCaliper is a service level solution that acts as a "proxy" or trasducer service, between a data collection/generator or "producer" end-point and a data consumption or "consumption" end-point, to transform any producer xAPI data collection API generated events to an IMS Caliper standardard JSON form stream received by the consumption end-point. This enables producer end-points to generate events in either using the xAPI statement and profile/"recipe" API, or the more semantic learning activity modeled IMS Caliper API, with all events serialized into a common IMS Caliper standard JSON formed stream transmitted to a target consumer end-point (i.e. LRS, RT analytics service etc).  Since the IMS Caliper event model and JSON structure is sufficiently equipped to support both xAPI's flexible non-standard events and Caliper's more semantically structured standardized events, this enables producers to utilize the data generation/collection API that is most effectice for their application while transmitting to consumption end-points as single IMS Caliper standard JSON stream for further processing.  Consumption end-points therefore remain opaque to and therefore need not specialize handling for any differing producer API utilized.  Applications that currently use the xAPI API to collect/generate data because it is sufficient for the end-to-end transmission needed, can continue to do so with the xCaliper service enabled if the consumption end-point is required also to handle IMS Caliper collected/generated events as well.  Also, if an xAPI producer application has additional requirements that warrant the usage of the more semantically enabled and extensible, learning activity modeled IMS Caliper API, this can easily be introduced by the producer without concern over having to transform respective JSON transmission streams to aiign with the consumption end-point targeted. 

#### Expected Market Impact for xCaliper
*\[Need to summarize the expected market facing impact of taking an initial step with xCapiper to first and foremost provide a unified "bridge" for xAPI to be supported as seamlessly and lossless as possible within the Caliper overarching event model and JSON stream framework.  In essense providing a compatibility for xAPI within Caliper so that producers and consumers can rely on the Caliper superset and standard as the unifying framework going forward especially when both xAPI and Caliper implementation support is required simultaneously to support the requiremens\]*

### The xCaliper Technical Solution
**_<<Suggest that we re-orient this section to effectively outline the technical solution proposed to leverage Caliper event model and JSON as the backbone superset to enable a bridge for supporting in parallel xAPI statement and "recipe"/profile generated / collected data transmitted via Caliper JSON to a Caliper supporting end-point >>_**

### xCaliper service behavior

xCaliper will provide a conversion service designed to transform xAPI statements into Caliper events.  Converted Statements will be expressed as JSON-LD.  Caliper terms will supercede xAPI terms whenever exact match and near match mappings exist.  In certain cases, xAPI Activity types and verbs will be retained in order to preserve Statement semantics across the conversion.  This may result in statement-to-event conversions that extend existing Caliper controlled vocabularies.      
 
#### Message Header processing
xCaliper will process both PUT (single statement) and POST (single statement, batch statements) HTTP requests.  Message headers will be inspected, in particular `Content-Type` and the custom `X-Experience-API-Version` request headers.  xAPI message requests normally set the `Content-Type` value to "application/json".  Statement requests that include Attachments use the "multipart/mixed" content type.

Regarding `Content-Type` handling xCaliper should consider adopting behaviors that mirror LRS requirements as described in the xAPI Spec, sections [1.5.1](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Communication.md#15-content-types) and [1.5.2](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Communication.md#152-multipartmixed):

"application/json" document types

* accept PUT or POST requests that include Statement batches containing either no Attachment objects or Attachment objects with a defined fileURL.

"multipart/mixed" document types

* accept PUT or POST requests that include Statement batches containing Attachments in the transmission format described in the xAPI spec.

* reject PUT or POST requests that include Statement batches that include Attachments that lack a defined `fileURL` or fail to match a received Attachment part based on their hash.

* assume a Content-Transfer-Encoding of binary for Attachment parts when receiving a PUT or POST requests.

* reject Statement batches that are larger than xCaliper is configured to allow.

* accept batches of Statements which contain no Attachment Objects when receiving a PUT or POST requests.

* accept batches of Statements which contain only Attachment Objects with a populated `fileUrl` when receiving a PUT or POST requests.

xCaliper will process 1.0.x Statements only.  The service will reject requests without a custom xAPI version header (pre xAPI version 0.95) or with a version header set to a value other than "1.0.0" unless such requests are routed to a fully conformant implementation of the prior version specified in the header (see xAPI Spec [2.4.10](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#2410-version) and [3.3](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Communication.md#versioning)).

#### Discerning Event types
Mapping an xAPI Statement to a Caliper `Event` subtype such as a `MessageEvent` is not a straightforward operation.  Unlike Caliper, which is explicit in its use of `Event` subtypes for descriptive purposes and as an aid to querying (e.g., `AssessmentEvent`, `MessageEvent` etc.), xAPI Statement "types" are defined optionally by specifying a `context.contextActivities` object typed with an array of one or more "category" object values.  Each category is defined as "an Activity used to categorize the Statement".  The array can be used to link a Statement to a profile such as cmi5 (see cmi5 9.6.2 [contextActivities](https://github.com/AICC/CMI-5_Spec_Current/blob/quartz/cmi5_spec.md#context_activities). 

However, xAPI profiles and receipes are neither uniformaly consistent in design nor referenced consistently in xAPI statments.  The xAPI spec provides an example of a Statement referencing cmi5 `contextActivities` categories (see xAPI Spec [Appendix B: cmi5](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-About.md#appendix-b-cmi5-example).  xCaliper should be able to leverage this information when attempting to map a Statement to a Caliper `Event` type.  On the other hand, the JISC receipes do not utilize `context.contextActivities` to link its xAPI Statements to a particular receipe choosing instead to provide a generic receipe reference in `context.extensions` (see JISC [logged in example](https://github.com/jiscdev/xapi/blob/master/vle/blackboard/loggedin.json).  These sorts of Statement variations may prove challenging when attempting to poll xAPI profiles and receipes for hints.

Statement "types" can also be inferred from an Activity Object Definition (see xAPI Spec [2.4.4.1](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#activity-definition).  If an xAPI `object` is typed as an `Activity` it may include an optional activity `definition` object property that provides additional metadata regarding the activity including a recommended `type` IRI.  If a `definition.type` value is provided xCaliper could use it to map the Statement to a Caliper `Event` type.  

```
{
  "object": {
    "id": "http://moodle.data.alpha.jisc.ac.uk/course/view.php?id=4",
    "definition": {
      "type": "http://adlnet.gov/expapi/activities/assessment",
      . . .
    },
    "objectType": "Activity"
  }
}
```
to

```
{
  "@context": "http://purl.imsglobal.org/ctx/caliper/v1p1",
  "id": "urn:uuid:27734504-068d-4596-861c-2315be33a2a2",
  "type": "AssessmentEvent",
  . . .
  "object": {
    "id": "http://moodle.data.alpha.jisc.ac.uk/course/view.php?id=4",
    "type": "Assessment"
    . . .
  }
}
```

There are other ways to map an xAPI Statement to a Caliper `Event` type.  Certain Statement `verb` choices such as "annotated", "logged-in" or "logged-out" should prove sufficient to map a Statement to a particular `Event` type (e.g., `AnnotationEvent`, `SessionEvent`).  Other mappings may require more complex associations involving Statement `verb`, the `object`, and other property combinations.  For instance, if a Statement includes a `result` in combination with a `verb` with a value of "completed", "earned", "failed", "graded", "passed" or "scored" xCaliper could infer that the Statement is scoring related and map it to a Caliper `GradeEvent`.  xCaliper may well need to employ a duck typing strategy to map these sort of associations.

#### Statement `id` processing
xAPI Statement providers are not required to provide a UUID identifer (the LRS must set it if not provided).  If a UUID is provided xCaliper will map it to `Event.id` as a URN using the form `urn:uuid:<UUID>`.  If the Statement is not provisioned with a UUID xCaliper will generate and assign a UUID , perhaps with a reference to UUID assignment in a changeLog object in `Event.extensions.` 

```
{
  "id": "urn:uuid:1b557176-ba67-4624-b060-6bee670a3d8e",
  "type": "Event",
  . . .
  "extensions": {
    "changeLog": {
      "dateConverted": "2017-11-18T11:59:59.000Z",
      "idAssignedBy": "xCaliper | Provider"
    }
  }
}
```

#### Statement `actor` processing
An xAPI Statement `actor` can be an `Agent` or a `Group` (a collection of type `Agent`).  xCaliper will map the xAPI Statement `actor` to the Caliper `Event.actor` typed as a Caliper `Person` or `Group`.

```
{
  "actor": {
    "mbox": "mailto:example.learner@example.com",
    "name": "Example Learner",
    "objectType": "Agent"
  }
}
```

to

```
{
  "actor": {
    "id": "mailto:example.learner@example.com",
    "type": "Person",
    "name": "Example Learner"
  }
}
```

Each xAPI `Agent` includes a required "inverse functional identifier" comprising either an mbox mailto IRI (email address), hex-encoded SHA1 hash of a mbox mailto IRI, OpenId or an account object that represents a user account on an existing system such as an LMS.  Given that an xAPI `Agent` can be assigned a non-IRI identifier we will need to establish a conversion rule for xCaliper when such identifiers are encountered.  For example, an xAPI `account.name` object can be assigned an otherwise opaque value representing a unique login id or name.

```
{
  "actor": {
    "account": {
      "homePage": "http://www.example.com",
      "name": "1625378"
    },
    "objectType": "Agent"
  }
}
```

```
{
  "actor": {
    "id": "???",
    "type": "Person",
    "extensions": {
      "xapi": {
        "account": {
          "homePage": "http://www.example.com",
          "name": "1625378"
        }
      }
    }
  }
}
``` 

#### Statement `verb` processing
The absence of firm xAPI governance and curation practices means that anyone can mint a verb for use in an xAPI statement.  That said, Rustici has made an attempt to provide the xAPI community with a registry of xAPI verbs.  Most of the "Tincan" verbs are drawn from the W3C Activity Streams 1.0 specification, a verb set much reduced in size in the re-scoped Activity Streams 2.0 release.  ADL's set of xAPI verbs are also included.  Rustici has itself contributed a robust set of verbs to the registry.  A small set of additional verbs are drawn from a variety of commercial providers including Brindleway, HT2 Labs (Curatr), RISC and Andrew Downes among others.  Not all verbs in the Rustici registry focus on learning (e.g., "laughed", "purchased", "ran", "walked"). 

| &nbsp; | Caliper | ADL xAPI | Rustici xAPI | Activity Streams 2.0 | Activity Streams 1.0 | Others |
| :----- | :-------| :------- | :----------- | :------------------- | :------------------- | ----- |
| Total  | 64 | 30 | 46 | 28 | 88 | 19 |

Where equivalencies exist between the various verb vocabularies xCaliper can simply map the term to the appropriate action, as the following example illustrates:

```
{
  "verb": {
    "id": "https://w3id.org/xapi/adl/verbs/logged-in",
    "display": {
      "en-US": "logged in"
    }
  }
}
```

```
{
  "action": "LoggedIn"
}
```

However, there are many xAPI verb object representations that have no ready equivalent in Caliper.  Consider ADL's list of 30 xAPI verbs.  As of today, we can map 13 ADL verbs (43.33%) to Caliper actions.  Of the 13 verbs we can achieve an exact match on 8 (26.66%) and a near match on 5 (16.66%), such as converting ADL's "updated" to Caliper's "modified".  In such cases, xAPI verb IRIs will be need to be assigned as values: 

```
{
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/attended",
    "display": {
      "en-US": "attended"
    }
  }
}
```

```
{
  "action": "http://adlnet.gov/expapi/verbs/attended"
}
```

As the Caliper information model evolves the number of defined actions will increase.  For instance, the draft Caliper Digital Badges profiles adds a dozen new actions.  Mapping xAPI verbs to Caliper actions will require the establishment and maintenance of data dictionaries or term mappings.  Defining and publishing equivalencies between verbs and actions (using the SKOS vocabulary [mapping properties](https://www.w3.org/TR/skos-reference/#mapping)) as well as Entities and xAPI activity types should be included in the scope of the proposed IMS "Profiles Registry".  Indeed, we recommend that IMS propose to Rustici that they consider retiring their commercial-backed registry in favor of an IMS-sponsored replacement.

#### Statement `object` processing
Likewise, various xAPI vocabularies define Activity types for use when the object of a Statement is an "Activity" rather than an `Agent`, `Group`, `Substatement` or `Statement Reference`.  Despite the confusing nomenclature an xAPI `Activity` is equivalent to a Caliper `Entity`.  Each xAPI Activity type is provisioned with a required `id` (type = IRI) and optional `objectType` (type = string of value "Activity") and `definition` object.  The `definition` or "Activity Definition Object" provides additional recommended and optional metadata about the Activity (see xAPI spec [2.4.4.1](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#2441-when-the-objecttype-is-activity)).

The Rustici TinCan registry defines 110 Activity Types drawn principally from Rustici, W3C Activity Streams 1.0, and ADL.  A small set of additional verbs are drawn from a variety of commercial providers including Brindleway, HT2 Labs (Curatr), RISC and Andrew Downes among others.  In addition, W3C Activity Streams 2.0 defines 8 Core types, 5 Actor types, 12 Object types, and 1 Link Type.

| &nbsp; | Caliper | ADL xAPI | Rustici xAPI | Activity Streams 2.0 | Activity Streams 1.0 | Others |
| :----- | :-------| :------- | :----------- | :------------------- | :------------------- | ----- |
| Total  | 44 | 16 | 58 | 26 | 28 | 8 |

Not all the Activity Types listed in the Rustici Tincan registry describe learning-related objects (e.g., "sales opportunity", "security role", "changed diaper").

As of Caliper 1.1 we can map a minimum 48 xAPI Activity Types (43.63%) listed in the Rustici Tincan Registry directly to Caliper Entity sub types.  As an example, ADL xAPI Activity types would likely be mapped to Caliper Entities as follows:  

| Caliper | ADL xAPI | Notes |
| :-------| :------- | :---- | 
| Assessment | assessment | &nbsp; |
| Attempt | attempt | &nbsp; |
| CourseOffering | Course |  &nbsp; |
| Entity | file | &nbsp; |
| Entity | interaction | &nbsp; |
| AssignableDigitalResource | lesson | &nbsp; |
| DigitalResource | link | &nbsp; |
| DigitalResource | media | &nbsp; |
| Entity | meeting | &nbsp; |
| AssignableDigitalResource | module |
| LearningObjective | objective | &nbsp; |
| Entity | profile | &nbsp; |
| AssessmentItem | question | &nbsp; |
| Entity | simulation |  &nbsp; |

xAPI Activity Types will be mapped as follows:

If `object.objectType` = "Activity", map the xAPI Statement `object` to a Caliper `Entity`.  If possible, convert the xAPI Activity type to an `Entity` subtype if a known equivalency exists.  xCaliper would attempt to convert Activity `definition` object properties to Caliper `Entity` properties (e.g., `name` to `name`). xAPI properties that could not be mapped to an existing Caliper property would be added to `Entity.extentions`.

```		  
{
  "object": {
    "id": "https://www.example.com/assessments/5",
    "definition": {
      "name": {
        "en-US": "Assessment no. 5"
      },
      "type": "http://adlnet.gov/expapi/activities/assessment"
    },
    "objectType": "Activity"
  }
}
```

to

```
{
  "object": {
    "id": "https://www.example.com/assessments/5",
    "type": "Assessment",
    "name": "Assessment no. 5"
  }
}
```	

If an xAPI Activity type is not described by the Caliper model xCaliper could either retain the original type by defining for it an inline @context or map it as a generic `Entity` and record the xAPI Activity Object Type in `Entity.extensions` (Example 3).
	
```
{
  "object": {
    "id": "https://www.example.com/simulations/1",
    "definition": {
      "name": {
        "en-US": "Simulation no. 1"
      },
      "type": "http://adlnet.gov/expapi/activities/simulation"
    },
    "objectType": "Activity"
  }
}
```

to

```
{
  "object": {
    "@context": {
      "adl": "http://adlnet.gov/expapi/activities/",
      "simulation": "adl:simulation"
    },
    "id": "https://www.example.com/simulations/1",
    "type": "simulation",
    "name": "Simulation no. 1"
  }
}
```

```
{
  "object": {
    "id": "https://www.example.com/meetings/27",
    "definition": {
      "name": {
        "en-US": "xCaliper Meeting"
      },
      "type": "http://adlnet.gov/expapi/activities/meeting"
    },
    "objectType": "Activity"
  }
}
```

converted to

```
{
  "object": {
    "id": "https://www.example.com/meetings/27",
    "type": "Entity",
    "name": "xCaliper Meeting",
    "extensions": {
      "xapi": {
        "object": {
          "definition": {
            "type": "http://adlnet.gov/expapi/activities/meeting"
          }
        }
      }
    }
  }
}
```

If `object.objectType` = "Agent" or "Group", xCaliper would map the xAPI Statement `object` to a Caliper `Person`, `Group` as described above. 

If `object.objectType` = "StatementRef" xCaliper can either map the xAPI Statement `object` to a generic Caliper `Event` or retain the xAPI type and provide an inline @context based on Jason Haag's draft [xAPI ontology](https://github.com/arwhyte/xapi-ontology/blob/master/ontology.rdf).  Note that an xAPI StatementRef object is provisioned with only an `id` and `type`.  If xCaliper were to convert a StatementRef to a generic Caliper `Event` certain required properties (i.e., `actor`, `action`, `object`, `endTime`) could not be set--a violation of the current model.

```
{
  "object": {
    "objectType": "StatementRef",
    "id": "8f87ccde-bb56-4c2e-ab83-44982ef22df0"
  }
}
```
to

```
{
  "object": {
    "id": "urn:uuid:8f87ccde-bb56-4c2e-ab83-44982ef22df0",
    "type": "Event"
  }
}
```

or

```
{
  "object": {
    "@context": {
      "xapi": "https://w3id.org/xapi/ontology#",
      "StatementRef": "xapi:StatementRef"
    },
    "id": "urn:uuid:8f87ccde-bb56-4c2e-ab83-44982ef22df0",
    "type": "StatementRef"
  }
}
```

Similarly, if `object.objectType` = "SubStatement" xCaliper can either map the xAPI Statement `object` to a generic Caliper `Event` or retain the xAPI type and provide an inline @context based on Jason Haag's draft [xAPI ontology](https://github.com/arwhyte/xapi-ontology/blob/master/ontology.rdf).  The xAPI SubStatement should include values for all required Caliper `Event` properties. 

#### Statement `result` processing
Unlike Caliper, xAPI provides its Statement with a top-level `result` property.  This sort of privileging is understandable given xAPI's SCORM antecedent and roots in corporate training where compliance is a key goal.  A Caliper `Result` is but one of a number of "generated" entities that may be produced during an interaction such as grading an assignment.  When xCaliper encounters an xAPI Statement `Result` it will map it to `Event.generated`.

\[TODO: add JSON examples\]

#### Statement `context` processing

\[TODO\]
	
#### Statement `timestamp` processing
xAPI Statement providers are not required to provide a timestamp (the LRS must set it if not provided).  If a timestamp is provided xCaliper will map it to `Event.eventTime` as an ISO 8601 formatted date time string value.  If the Statement is not provisioned with a timestamp xCaliper will generate and assign an `eventTime`, perhaps with a reference to the date time assignment in a changeLog object in `Event.extensions.` 

```
{
  "id": "urn:uuid:1b557176-ba67-4624-b060-6bee670a3d8e",
  "type": "Event",
  . . .
  "eventTime": "2017-11-18T11:59:59.000Z",
  "extensions": {
    "changeLog": {
      "dateConverted": "2017-11-18T11:59:59.000Z",
      "eventTimeAssignedBy": "xCaliper | Provider"
    }
  }
}
```
	
#### Statement `stored` processing
The xAPI Statement `stored` timestamp property is set by the LRS upon receipt of the Statement.  xCaliper will not provide a `stored` date time value in `Event.extensions`.

#### Statement `authority` processing
The xAPI Statement `authority` object property provides a reference to an Agent or a Group representing a 3-legged OAuth asserting the veracity of the Statement (see xAPI Spec [2.4.9](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#249-authority).  Although listed as optional an `authority` must be set by the LRS if not provided or if a strong trust relationship between the Statement Provider and LRS has not been established.  In the case of a 3-legged OAuth, the `authority` is a `Group` composed of two Agents.  The `Agent` that represents the OAuth consumer must be identified by an `account` while the second `Agent` represents the user. 
	
if xCaliper encounters a Statement `authority` the object will be mapped to `Event.extentions`.  If no `authority` object value is provided xCaliper will ignore the property.

```
{
  "authority": {
    "objectType": "Group",
    "member": [{
        "account": {
          "homePage": "http://example.com/xAPI/OAuth/Token",
          "name": "oauth_consumer_x75db"
        }
      },
      {
        "mbox": "mailto:bob@example.com"
      }
    ]
  }
}
```		
	
to

\[TODO: represent `authority` with an inline context or convert to a Caliper `Agent` or `Group`? \]

```
{
  "extensions": {
    "xapi": {
      "authority": {
         . . .
      }
    }
  }
}	
```	
	
#### Statement `version` processing
The xAPI spec does not recommend setting the Statement `version` property since versioning is handled by a custom Header.  If a Statement provider sets the Statement `version` it must set the value to "1.0.0" rather than the latest patch version.

xCaliper will not set the `version` property.  If a Statement provider chooses to set the `version` to "1.0.0" xCaliper will map it to `Event.extensions`.  If xCaliper encounters a Statement with a `version` set to a value other than "1.0.0" it will reject the Statement.

```
{
  "version": "1.0.0"
}
```

to

```
{
  "extensions": {
    "xapi": {
      "version": "1.0.0"
    }
  }
}
```
	
#### Statement `attachments` processing

\[TODO: describe \]	

### Appendix A.  xAPI Statement to Caliper Event mappings

| xAPI | Type | Conformance | Caliper | Type | Conformance | Notes |
| :--- | :--- | :---------- | :------ | :--- | :---------- | :---- |
| `id` | UUID | Required | `Event.id` | UUID URN | Required | Remap `Statement.id` as urn:uuid\<UUID\>. |
| `actor` | Object | Required | `Event.actor` | Agent | Required | &nbsp; |
| `verb` | Object | Required | `Event.action` | Term | Required | Remap to `Event.action` and/or adjust Caliper to express an action as either an Object, string IRI or string term.  We can alias the JSON-LD @language keyword in order to map the xAPI `display` property. | 
| `object` | Object | Optional | `Event.object` | `Entity` | Required | &nbsp; |
| `result` | Object | Optional | `Event.generated` | `Result` | Optional | &nbsp; |
| `context` | Object | Optional | `Event.extensions.xapi.context` | Object | Optional | Certain `context` properties can be mapped to Caliper properties.  See below. |
| `context.registration` | UUID | Optional | `Event.extensions.xapi.context.registration` | UUID | Optional | &nbsp; |
| `context.instructor` | Agent or Group | Optional | `Organization.extensions` | `Person` or `Group` | Optional | Caliper: assuming `Event.group` is a `Course` or `CourseSection` map to `Course.extensions` or `CourseSection.extensions`. |
| `context.team` | Group | Optional | `Event.membership.organization` | Membership | Optional | Caliper: map to `Event.membership.organization` (although xAPI does not appear to model an individual member's role or status) or `Event.extensions`. |
| `context.contextActivities` | ContextActivities Object | Optional | `Event.extensions.xapi.context.contextActivities` | `Entity` | Optional | Additionally, if type = "parent" and Statement `object` is an Activity and `Event.object` is a `DigitalResource` then map to `DigitalResource.isPartOf`.  If type=grouping then map to `Event.group` `Organization.subOrganizationOf`.  A link to an xAPI profile can be established by defining a "category" type. |
| `context.revision` | string | Optional | `Event.object` | `DigitalResource.version` | Optional | xAPI: use only if object is an Activity). Map to `DigitalResource.version` or `extensions.xapi.context.revision`. |
| `context.platform` | string | Optional | `Event.extensions.xapi.platform` | string | Optional | Caliper: ideally `context.platform` should map to `Event.edApp` but since there is no xAPI requirement that the value be expressed as an identifier we may need to simply map it to `Event.extenstions`. |
| &nbsp; | &nbsp; | &nbsp; | `Event.edApp` | SoftwareOrganization | Optional | &nbsp; |
| &nbsp; | &nbsp; | &nbsp; | `Event.group` | Organization | Optional | &nbsp; |
| `context.language` | string | Optional | `Event.extensions.xapi.language` | string | Optional | xAPI: RFC 5646 language code. |
| `context.statement` | Statement Reference Object | Optional | `Event.extensions.xapi.statement` | Object | Optional | xAPI: another Statement considered as context for this Statement. |
| `context.extensions` | Object | `Event.extensions` | Object | xAPI: A map of other domain-specific context relevant to the Statement. Caliper: map to `Event.extensions`. |
| `timestamp` | DateTime | Optional | `Event.eventTime` | DateTime | Required | Caliper: map to `Event.eventTime` if `timestamp` is provided by Statement provider or set it (add an extension property that indicates that the consumer set the `eventTime`. |
| `stored` | DateTime | Optional | &nbsp; | &nbsp; | &nbsp; | xAPI: set by LRS so no Caliper mapping is required. |
| `authority` | Object | Optional | `Event.extensions.xapi.authority` | Object | Optional | Caliper: no equivalent property currently described. |
| `version` | string | Not recommended | `Event.extensions.xapi.version` | string | Optional | Caliper: Events and Entities are versioned via reference to the JSON-LD @context and the `Envelope.dataVersion` property. | 
| `attachments` | Array\<Object\> | Optional | `Event.extensions.xapi.attachments` | Array\<Object\> | Optional | xAPI: ordered object array of headers for Attachments to the Statement. |

#### Notes

`object`.  See xAPI Spec [2.4.4](https://github.com/adlnet/xAPI Spec/blob/master/xAPI-Data.md#244-object).

`context.registration`.  "When an LRS is an integral part of an LMS, the LMS likely supports the concept of registration. The Experience API applies the concept of registration more broadly. A registration could be considered to be an attempt, a session, or could span multiple Activities. There is no expectation that completing an Activity ends a registration. Nor is a registration necessarily confined to a single Agent.

The Registration is also used when storing documents within the State Resource, e.g. for bookmarking. Normally the same registration is used for requests to both the Statement and State Resources relating to the same learning experience so that all data recorded for the experience is consistent."  See xAPI Spec [2.4.6.1](https://github.com/adlnet/xAPI Spec/blob/master/xAPI-Data.md#2461-registration-property).

`context.instructor`.  Instructor associated with the Statement if not already defined as the Statement `actor`. See xAPI Spec [2.4.6](https://github.com/adlnet/xAPI Spec/blob/master/xAPI-Data.md#246-context).

`context.team`.  Team associated with the Statement if not already defined as the Statement `actor`.  See xAPI Spec [2.4.6](https://github.com/adlnet/xAPI Spec/blob/master/xAPI-Data.md#246-context).

`context.contextActivities`.  A map of learning activity context types associated with the Statement.  

> There are four valid context types. All, any or none of these MAY be used in a given Statement:
>
> Parent: an Activity with a direct relation to the Activity which is the Object of the Statement. In almost all cases there is only one sensible parent or none, not multiple. For example: a Statement about a quiz question would have the quiz as its parent Activity.
>
> Grouping: an Activity with an indirect relation to the Activity which is the Object of the Statement. For example: a course that is part of a qualification. The course has several classes. The course relates to a class as the parent, the qualification relates to the class as the grouping.
>
> Category: an Activity used to categorize the Statement. "Tags" would be a synonym. Category SHOULD be used to indicate a profile of xAPI behaviors, as well as other categorizations. For example: Anna attempts a biology exam, and the Statement is tracked using the cmi5 profile. The Statement's Activity refers to the exam, and the category is the cmi5 profile.
>
> Other: a contextActivity that doesn't fit one of the other properties. For example: Anna studies a textbook for a biology exam. The Statement's Activity refers to the textbook, and the exam is a contextActivity of type other."

See xAPI Spec [2.4.6.2](https://github.com/adlnet/xAPI Spec/blob/master/xAPI-Data.md#2462-contextactivities-property).

`context.revision`. The "revision" property MUST only be used if the Statement's Object is an Activity; The "revision" property SHOULD be used to track fixes of minor issues (like a spelling error); The "revision" property SHOULD NOT be used if there is a major change in learning objectives, pedagogy, or assets of an Activity. (Use a new Activity id instead).  See xAPI Spec [2.4.6](https://github.com/adlnet/xAPI Spec/blob/master/xAPI-Data.md#246-context).

`context.platform`. See xAPI Spec [Appendix A](https://github.com/adlnet/xAPI Spec/blob/master/xAPI-Data.md#appendix-a-example-statements) or a JISC [example](https://github.com/jiscdev/xapi/blob/master/recipes/assignment-submitted.md). 

`context.language`.  RFC 5646 code "representing the language in which the experience being recorded in this Statement (mainly) occurred in, if applicable and known."  See xAPI Spec [2.4.6](https://github.com/adlnet/xAPI Spec/blob/master/xAPI-Data.md#246-context).

`context.statement`.  Another Statement to be considered as context for this Statement.  See xAPI Spec [Statement References](https://github.com/adlnet/xAPI Spec/blob/master/xAPI-Data.md#statement-references).


#### Appendix B. xAPI Activity Type to Caliper Entity mappings

| xAPI | Type | Conformance | Caliper | Type | Conformance | Notes |
| :--- | :--- | :---------- | :------ | :--- | :---------- | :---- |
| `id` | UUID | Required | `Entity.id` | IRI | Required | Remap Activity identifier as urn:uuid\<UUID\>. |
| `objectType` | string | Optional | &nbsp; | &nbsp; | &nbsp; | Utilize `definition.type` or type as a generic Caliper `Entity`. |
| `definition` | Object | Optional | &nbsp; | &nbsp; | &nbsp; | Caliper: map attributes to various `Entity` properties. |
| `definition.type` | IRI | Recommended | `Entity.type` | IRI | Required | If a mapping exists between the type and a Caliper `Entity` type use the Caliper type; otherwise use generic `Entity` type and add `Entity.extensions.xapi.definition.type`. |
| `definition.name` | Language Map | Recommended | `Entity.name` | string | Optional | Caliper: either ignore the language map or store the key (e.g., "en-US") as `Entity.extensions.xapi.definition.name.key.languageTag`. |
| `definition.description` | Language Map | Recommended | `Entity.description` | string | Optional | Caliper: either ignore the language map or store the key (e.g., "en-US") as `Entity.extensions.xapi.definition.description.key.languageTag`. |
| `definition.moreInfo` | IRL | Optional | `Entity.extensions.xapi.definition.moreInfo` | &nbsp; | &nbsp; | "Resolves to a document with human-readable information about the Activity, which could include a way to launch the activity."|
| `definition.extensions` | Object | Optional | `Entity.extensions` | Object | Optional | &nbsp; |