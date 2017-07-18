### Background for the Proposed "xCaliper" Solution
#### Premise
* There is clearly no benefit to having two overlapping edu focused, RDF triple event structured, data collection APIs out there.
 
* xAPI, because of its flexibility and non-standard conformance approach to collecting different, endpoint agreed upon, custom streams of data, and the availability in the market prior to Caliper, has gotten some early adoption traction in the edu space.
 
* A majority of xAPI usage focuses on non-standard, custom endpoint agreed upon exchanges of event streams consisting of more basic event data.

* xAPI "profiles" or "recipes" are fairly new and do not cover the entire learning activity spectrum in detail or in an extensible,  semantically modeled form, but are a useful mechanism for aggregating a collection of data elements for a more well defined and consistent target scenario such as Assessment, Video etc.

* Caliper's design premise and implementation is to build a standardized event-based data model. This model is optimized for collecting deep / granular learning activity interactions with extensibility and a common education focused vocabulary and ontology. With that, the Caliper model covers, in detail, learning activity and related context with a consistent, standardized form via the baseline event structure and a related set of "metric profiles" representing individual learning activities and all related context.  The primary goal is that all learning activities sourced from a variety of providers adhere to a common standard for representing and sharing all of their detailed learning activity data.

* The Caliper event model can also support, much like xAPI, a very basic event encapsulation of more non-standard, customized endpoint agreed upon data streams as required.  However, although flexible and useful, this does not foster the interoperability of both basic and more deep learning activity data that Caliper is primarily focused on in its well defined and structured event model.

#### xCaliper Assumptions and Rationale

* The xAPI statement level API and event model is primarily intended and used to date in order to package up and transmit a wide spectrum of customized, non-standardized data streams between a source collection/provider endpoint and a target storage/consumer endpoint.  This fosters the much used flexibility at the expense of standardizing the stream structure and data streams per activities and other educational context entities and workflows. xAPI Recipes have recently been introduced just to begin to define some common and more esoteric "profiles" of data streams that in aggregate have a more well-defined structure and composition for certain activity based scenarios.

* Recipe use and true standardization is minimal to date with the large majority of xAPI usage applied to more basic, shallow and customized data collection between endpoints.

* Caliper's ability to support both a basic, non-standard custom data stream as well as its metric profile based more well defined and extensive learning activity event model, makes it highly suitable to be a superset to also represent both xAPI statement level API structured data streams as well as its predefined "recipe" based aggregates.  It would be impossible to consider the inverse of xAPI as a superset as the Caliper API and underlying event model has a more well defined, semantically rich, common vocabulary/ontology representation that would be difficult to fold into xAPI without substantial loss of fidelity.

* Caliper's resulting JSON-LD representation can be a single standardized stream representation to be consumed by any endpoint regardless of whether the xAPI or Caliper APIs were utilized for collection and transmission. This does require xAPI and Caliper supporting consumption endpoints to preferably support the single xCaliper unified JSON-LD representation (as long as the xCaliper service is enabled when both xAPI and Caliper collection endpoints are in play), or worst case continue to support the xAPI standalone JSON as well as adding support for the xCaliper unified JSON-LD stream.

* Since the majority of xAPI usage is for non-standard, custom, and more shallow data exchanges between agreed upon endpoints, with little / no standardization across the same learning activity types and contexts, this supports via this proposal for xCaliper to have Caliper act as the superset API and representation of the collection data streams.

* However, with that, those applications that are xAPI enabled, and whose requirements are satisfied by a more basic data exchange, can continue to utilize the xAPI collection endpoint API to package and transmit events.  The xCaliper service "proxy" will be enabled to transduce the xAPI collection API emitted raw stream to Caliper superset JSON-LD prior to the target / storage endpoint receiving the stream in Caliper JSON-LD form.

* As applications requirements broaden with the need for more standardized, consistent, deep learning analytics data across multiple learning activities regardless of where these are sourced, the Caliper API can be used for data collection to support these more detailed use cases

* If the xAPI initial set of recipes, can be mapped and migrated to Caliper metric profiles then both the extensive IMS Caliper community and the xAPI community can continue to collectively work on expanding metric profiles to cover more and more learning activity use case scenarios.

#### What is xCaliper ?

xCaliper is a service level solution that acts as a "proxy" or transducer service between a data collection/generator or "producer" endpoint and a data consumption or "consumption" endpoint.  xCaliper will transform producer generated xAPI data statements to an IMS Caliper JSON-LD form stream received by the consuming endpoint. This enables producer endpoints to generate events using the xAPI statement and profile/"recipe" API or the more semantically-rich learning activity modeled IMS Caliper API, with all events serialized into a common IMS Caliper standard JSON-LD formed stream transmitted to a target consumer endpoint (i.e. LRS, RT analytics service, etc).
  
Since the IMS Caliper event model and JSON-LD structure is sufficiently equipped to support both xAPI's flexible non-standard events and Caliper's more semantically structured standardized events, this enables producers to utilize the data generation/collection API that is most effective for their application while transmitting to consumption endpoints as single IMS Caliper standard JSON stream for further processing. Consumption endpoints therefore remain opaque to and therefore need not provide specialize handling for any differing producer API utilized.  Applications that currently use the xAPI API to collect/generate data because it is sufficient for the end-to-end transmission needed, can continue to do so with the xCaliper service enabled if the consumption endpoint is required also to handle IMS Caliper collected/generated events as well.  Also, if an xAPI producer application has additional requirements that warrant the usage of the more semantically enabled and extensible, learning activity modeled IMS Caliper API, this can easily be introduced by the producer without concern over having to transform respective JSON transmission streams to align with the consumption endpoint targeted.
 

#### Expected Market Impact for xCaliper
xCaliper is intended to be an initial step, to first and foremost, provide a unified "bridge" for xAPI to be supported as seamlessly and lossless as possible within the Caliper overarching event model and JSON stream framework.  In essense providing a compatibility for xAPI within Caliper so that producers and consumers can rely on the Caliper superset and IMS certified standard as the unifying framework going forward, especially when both xAPI and Caliper implementation support may ofen be required simultaneously to support the requiremens.  Furthermore, this initial xCaliper implementation and ongong enhancements will enable both the Caliper and xAPI development communities to work more closely together to synchronize on individual efforts to make incremental improvements to educational analytics activity modeling and instrumentation as well as to work together on joint initiatives to create a single, consistent treatment for further expansion of learning activity event modeling via common vocabulary and ontology within the context of the Caliper Event Model framework.

During the initial cycle of xCaliper service implementaiton, the expectation is there will continue to be independent work and adoption in respective Caliper and xAPI communities, but once xCaliper is completed in its initial form there will be a viable solution in the market for the increasing number of analytics implementations that need to support both Caliper and xAPI based data collection within their institution's academic enterprise.  The details of how the respective IMS and xAPI development workgroups and communities will begin to work together needs to be addressed so as to create a more formalized and structured collaborative workgroup effort.

### The xCaliper Technical Solution
xCaliper will provide a conversion service designed to transform xAPI statements into Caliper events.  Caliper events will also be accepted and either routed directly to a target endpoint or validated before transmission elsewhere. Converted xAPI statements will be expressed as JSON-LD. Caliper terms will supercede xAPI terms whenever exact match and near match mappings exist. In certain cases, xAPI Activity types and verbs will be retained in order to preserve Statement semantics across the conversion. This may result in statement-to-event conversions that extend existing Caliper controlled vocabularies.

Mapping xAPI vocabularies to Caliper terms will require the establishment and maintenance of data dictionaries or mapping files.  Defining and publishing equivalencies between terms (perhaps using the SKOS vocabulary [mapping properties](https://www.w3.org/TR/skos-reference/#mapping)) should be included in the scope of the proposed IMS "Profiles Registry".  Indeed, we recommend that IMS propose to Rustici that they consider retiring their commercial-backed registry in favor of an IMS-sponsored replacement. 

What follows is a preliminary walkthrough of likely xCaliper behavior with respect to xAPI message header and statement property processing with example JSON / JSON-LD.     
 
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

#### Mapping Event types
Converting an xAPI Statement to a Caliper `Event` subtype such as a `MessageEvent` is not a straightforward operation.  Unlike Caliper, which is explicit in its use of `Event` subtypes for descriptive purposes and as an aid to querying (e.g., `AssessmentEvent`, `MessageEvent` etc.), xAPI Statement "types" are defined optionally by specifying a `context.contextActivities` array consisting of one or more `category` object values.  Each `category` is defined as "an Activity used to categorize the Statement".  The array can be used to link a Statement to a profile such as cmi5 (see cmi5 [9.6.2 contextActivities](https://github.com/AICC/CMI-5_Spec_Current/blob/quartz/cmi5_spec.md#context_activities). 

However, xAPI profiles and recipes are neither uniformly consistent in design nor referenced consistently in xAPI staterecipements.  The xAPI spec provides an example of a Statement referencing cmi5 `contextActivities` categories (see xAPI Spec [Appendix B: cmi5](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-About.md#appendix-b-cmi5-example).  xCaliper should be able to leverage this information when attempting to map a Statement to a Caliper `Event` type.  On the other hand, the JISC recipes do not utilize `context.contextActivities` to link its xAPI Statements to a particular recipe choosing instead to provide a generic recipe reference in `context.extensions` (see JISC [logged in example](https://github.com/jiscdev/xapi/blob/master/vle/blackboard/loggedin.json).  These sorts of Statement variations may prove challenging when attempting to poll xAPI profiles and recipes for hints.

In cases where an xAPI Statement resists conversion to a Caliper `Event` subtype, xCaliper will convert the Statement to a generic `Event`.  That said, there are other ways to map an xAPI Statement to a Caliper `Event` subtype.  Certain Statement `verb` choices such as "annotated", "logged-in" or "logged-out" should prove sufficient to map a Statement to a `AnnotationEvent` or a `SessionEvent`.  Statement "types" can also be inferred from the Activity Object Definition (see xAPI Spec [2.4.4.1](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#activity-definition).  If an xAPI `object` is typed as an `Activity` it may include an optional activity `definition` object property that provides additional metadata regarding the activity including a recommended `type` IRI.  If a `definition.type` value is provided xCaliper could use it to map the Statement to a Caliper `Event` type.  

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

Other mappings may require more complex associations involving Statement `verb`, the `object`, and other property combinations.  For instance, if a Statement includes a `result` in combination with a `verb` with a value of "completed", "earned", "failed", "graded", "passed" or "scored" xCaliper could infer that the Statement is scoring related and map it to a Caliper `GradeEvent`.  xCaliper may well need to employ a duck typing strategy to map these sort of associations.

#### Statement `id` processing
xAPI Statement providers are not required to provide a UUID identifier (the LRS must set it if not provided).  If a UUID is provided xCaliper will map it to `Event.id` as a URN using the form `urn:uuid:<UUID>`.  If the Statement is not provisioned with a UUID xCaliper will generate and assign a UUID , perhaps with a reference to UUID assignment in a changeLog object in `Event.extensions.` 

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
| :----: | :------: | :------: | :----------: | :------------------: | :------------------: | :---: |
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

As the Caliper information model evolves the number of defined actions will increase, enhancing xCaliper's ability to substitute Caliper actions for xAPI verbs.  For instance, the draft Caliper Digital Badges profile slated for Caliper 1.2 adds a dozen new actions.  However, there are many xAPI verb object representations that have no ready equivalent in Caliper.  Consider ADL's list of 30 xAPI verbs.  As of today, we can map 13 ADL verbs (43.33%) to Caliper actions.  Of the 13 verbs we can achieve an exact match on 8 (26.66%) and a near match on 5 (16.66%), such as converting ADL's "updated" to Caliper's "modified".  In such cases, xAPI verb IRIs will be need to be assigned as values: 

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

#### Statement `object` processing
Likewise, various xAPI vocabularies define Activity types for use when the object of a Statement is an "Activity" rather than an `Agent`, `Group`, `Substatement` or `Statement Reference`.  Despite the confusing nomenclature an xAPI `Activity` is equivalent to a Caliper `Entity`.  Each xAPI Activity type is provisioned with a required `id` of type IRI and optional `objectType` (type = string of value "Activity") and `definition` object.  The `definition` or "Activity Definition Object" provides additional recommended and optional metadata about the Activity (see xAPI spec [2.4.4.1](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#2441-when-the-objecttype-is-activity)).

The Rustici TinCan registry defines 110 Activity Types drawn principally from Rustici, W3C Activity Streams 1.0, and ADL.  A small set of additional verbs are drawn from a variety of commercial providers including Brindleway, HT2 Labs (Curatr), RISC and Andrew Downes among others.  In addition, W3C Activity Streams 2.0 defines 8 Core types, 5 Actor types, 12 Object types, and 1 Link Type.

| &nbsp; | Caliper | ADL xAPI | Rustici xAPI | Activity Streams 2.0 | Activity Streams 1.0 | Others |
| :----- | :------:| :------: | :----------: | :------------------: | :------------------: | :----: |
| Total  | 44 | 16 | 58 | 26 | 28 | 8 |

Not all the Activity Types listed in the Rustici Tincan registry describe learning-related objects (e.g., "sales opportunity", "security role", "changed diaper").

Whenever the xAPI Statement `object.objectType` = "Activity", xCaliper would convert the xAPI Statement `object` to a Caliper `Entity`.  As of Caliper 1.1 we can map a minimum 48 xAPI Activity Types (43.63%) listed in the Rustici Tincan Registry directly to Caliper Entity subtypes.  ADL describes fourteen Activity Types. Eight of these types (57.14%) can be mapped to available Caliper entities.  The remaining six Activity Types (42.85%) could either be mapped to the generic `Entity` or retain the original type and provide an inline JSON-LD context as described below.

If type equivalency can be established, xCaliper would convert the xAPI Activity type to an `Entity` subtype.  xCaliper would also attempt to convert Activity `definition` object properties to Caliper `Entity` properties (e.g., `name` to `name`). xAPI properties that resist mapping to an existing Caliper property would be added to `Entity.extensions`.

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

If no Caliper type exists that matches an xAPI Activity type xCaliper could retain the original type by defining for it an inline `@context` as part of the conversion process.
	
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

Otherwise, xCaliper could convert the unmatched xAPI Activity Type as a generic `Entity` and record the Activity Object Type in `Entity.extensions`.


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

to

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

If the xAPI Statement `object.objectType` = "Agent" or "Group", xCaliper would map the xAPI Statement `object` to a Caliper `Person`, `Group` as described above for converting the Statement `actor`. 

If the xAPI Statement `object.objectType` = "StatementRef" xCaliper can either map the xAPI Statement `object` to a generic Caliper `Event` or retain the xAPI type and provide an inline `@context` based on Jason Haag's draft [xAPI ontology](https://github.com/arwhyte/xapi-ontology/blob/master/ontology.rdf).  Note that an xAPI `StatementRef` object is provisioned with an `id` and `type` only.  If xCaliper were to convert a `StatementRef` to a generic Caliper `Event` certain required properties (i.e., `actor`, `action`, `object`, `endTime`) could not be set⎯a violation of the current Caliper model.

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

Similarly, if the `object.objectType` = "SubStatement" xCaliper can either map the xAPI Statement `object` to a generic Caliper `Event` or retain the xAPI type and provide an inline `@context` with a term drawn from Jason Haag's draft [xAPI ontology](https://github.com/arwhyte/xapi-ontology/blob/master/ontology.rdf).  The xAPI `SubStatement` should include values for all required Caliper `Event` properties. 

#### Statement `result` processing
Unlike Caliper, xAPI provides its Statement with a top-level `result` property.  This sort of privileging is understandable given xAPI's SCORM antecedent and roots in corporate training where compliance is a key goal.  The xAPI `Result` object is little more than a property bag without an `id` or a `type` specified which creates conversion challenges.  Despite Caliper modeling both a `Score` and a `Result` xCaliper may need to retain the xAPI `result` object as is and define an inline `@context` for it from terms drawn from Jason Haag's draft [xAPI ontology](https://github.com/arwhyte/xapi-ontology/blob/master/ontology.rdf).  The object could be recorded in `Event.generated` or `Event.extensions` if the former is deemed inappropriate since the object would not be typed as an `Entity`.

```
{
  "result": {
    "score": {
      "scaled": 0.95
    },
    "success": true,
    "completion": true,
    "duration": "PT1234S"
  }
}
```

to

```
{
  "generated": {
    "@context": {
      "xapi": "https://w3id.org/xapi/ontology#",
      "result": "xapi:Result",
      "score": "xapi:Score",
      "completion": "xapi:completion",
      "duration": "xapi:duration",
      "scaled": "xapi:scaled",
      "success": "xapi:success"
    },
    "result": {
      "score": {
        "scaled": 0.95
      },
      "success": true,
      "completion": true,
      "duration": "PT1234S"
    }
  }
}
```

#### Statement `context` processing

The xAPI Statement `context` property attempts to capture portions of the learning context in which a Statement is situated.  All xAPI `context` properties are considered optional.  xCaliper will copy the `context` as is to `Event.extensions` if provided.  Certain property values could be extracted from the Statement `context` and mapped to various Caliper `Event` properties as is noted below.  

| xAPI | Type | Description |
| :--- | :--- | :------------ |
| `context.registration` | UUID | The registration identifier of a Context object.  Copy to `Event.extensions` |
| `context.instructor` | Agent or Group | Copy to `Event.extensions.xapi.context`.  Assuming `Event.group` is a course the `instructor` could also be added to the `CourseOffering`/`CourseSection` `extensions` property. |
| `context.team` | Group | Copy to `Event.extensions`.  The `team` value could also be mapped to `Event.membership.organization` (although xAPI does not appear to model an individual member's role or status). |
| `context.contextActivities` | ContextActivities Object | Copy to `Event.extensions`.  Four context types are defined: "parent", "grouping", "category" and "other", each expressed as an array of values.  If a parent Activity is described and can be converted to a Caliper `DigitalResource` or `Organization` then map the parent to the Caliper `isPartOf` or `subOrganizationOf` property.  If a category Activity is described that links to an xAPI profile or recipe xCaliper may be able to map the Statement to a particular `Event` type based on the reference. |
| `context.revision` | string | `Event.extensions`.  If the Statement `object` is an Activity that can be mapped to a Caliper `DigitalResource` the `revision` value can also be mapped to `DigitalResource.version`. |
| `context.platform` | string | Copy to `Event.extensions`.  Unfortunately since there is no xAPI requirement that the `platform` value be expressed as an IRI there is no way to convert the value to a Caliper `SoftwareApplication` and map it to `Event.edApp`. |
| `context.language` | string | RFC 5646 language code.  Copy to `Event.extensions`. |
| `context.statement` | Statement Reference | Another Statement considered as context for this Statement. Copy to `Event.extensions`. |
| `context.extensions` | Object | A map of other domain-specific context relevant to the Statement.  Copy to `Event.extensions`. |

```
{
  "extensions": {
    "xapi": {
      "context": {
        "registration": "UUID",
        "instructor": {},
        "team": {},
        "contextActivities": {},
        "revision": "",
        "platform": "",
        "language": "",
        "statement": "",
        "extensions": {}
      }
    }
  }
}
```
	
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
	
If xCaliper encounters a Statement `authority` the object will be mapped to `Event.extensions`.  If no `authority` object value is provided xCaliper will ignore the property.

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