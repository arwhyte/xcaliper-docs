## Overview for Aligning Caliper and xAPI 
### Background for the Proposed "xCaliper" Solution
#### Premise
* There is clearly no benefit to having 2 overlapping edu focused, RDF triple event structured, data collection APIs out there
* xAPI, because of its flexibility and non-standard conforming approach to collect different, endpoint agreed upon, custom streams of data, and the availability in the market prior to Caliper, has gotten some early adoption traction in the edu space.
* A majority of xAPI usage focuses on non-standard, custom end-point agreed upon exchanges of event streams consisting of more basic event data streams.
* xAPI "profiles" or "recipes" are fairly new and do not cover the entire learning activity spectrum in detail or in an extensible,  semantically modeled form, but are a useful mechanism or aggregating a collection of data elements for a more well defined and consistent target scenario such as Assessment, Video etc.
* Caliper's design premise and implementation is to build a standardized event based data model. This model is optimized fo collecting deep / granular learning activity interactions with extensibility and a common education focused vocabulary and ontology. With that, the Caliper model covers, in detail, learning activity and related context via a consistent, standardized form via the baseline event structure and a related set of "metric profiles" representing individual learning activities and all related context.  The primary goal being that all learning activities sourced from a variety of providers can adhere to a common standard for representing and sharing all of their detailed learning activity data.
* The Caliper event model can also support, at the most basic level, much like xAPI, a very basic event encapsulation of more non-standard, customized endpoint agreed upon data streams as required.  However, although flexible and useful, this does not foster the interoperability of both basic and more deep learning activity data that Caliper is primarliy focused on in its well defined and structured event model.

#### xCaliper Assumptions and Rationale

* The xAPI statement level API and event model is primarily intended and used to date in order to package up and transmit a wide spectrum of customized, non-standardized data streams between a source collection/provider endpoint and a target storage/consumer endpoint.  This fosters the much used flexiblity at the expense of standardizing the stream structure and data streams per activities and other educational context entities and workflows. xAPI Recipes have recently been introduced just to begin to define some common and more esoteric "profiles" of data streams that in aggregate have a more well-defined structure and composition for certain activity based scenarios.

* Recipe use and true standardization is minimal to date with the large majority of xAPI usage applied to more basic, shallow and customized data collection between endpoints.

* Caliper's ability to support both a basic, non-standard custom data stream as well as its metric profile based more well defined and extensive learning activity event model, makes it highly suitable to be a super set to also represent both xAPI statement level API structured data streams as well as its predefined "recipe" based aggregates.  It would be impossible to consider the inverse of xAPI as a superset as the Caliper API and underlying event model has a more well defined, semantically rich, common vocabulary/ontology representation that would be difficult to fold into xAPI without substantial loss of fidelity.

* Caliper's resulting JSON representation can be a single standardied stream representation to be consumed by any endpoint regardless of whether the xAPI or Caliper APIs were utilied for colletion and transmission. This does required xAPI and Caliper supporting consumption endpoints to preferably support the single xCaliper unified JSON representation (as long as the xCaliper service is enabled if both xAPI and Caliper collection endpoints are in play), or worst case continue to support the xAPI standalone JSON as well as adding support for the  xCaliper unified JSON stream.

* Since the majority of xAPI usage is for non-standard, custom, and more shallow data exchanges between agreed upon endpoints, with little / no standardization across same learning activity types and contexts, this supports via this proposal for xCaliper to have Caliper act as the superset API and representation of the collection data streams.

* However, with that, those applications that are xAPI enabled, and whose requirements are satisified by a more basic data exchange, can continue to utliize the xAPI collection endpoint API to package and transmit events.  The xCaliper service "proxy" will be enabled to tranduce the xAPI collection API emitted raw stream to Caliper superset JSON prior to the target / storage endpoint receiving the stream in Caliper JSON form.

* As applications requirements broaden with the need for more standardized, consistent, deep learning analytics data across multiple learning activities regardless of where these are sourced, the Caliper API can be used fr data collection to support these ore detailed use cases

* The xAPI initial set of recipes, can be easiliy mapped and migrated to Caliper metric profiles and then both the extensive IMS Caliper community and the xAPI community can continue to collectively work on expanding metric profiles for cover more and more learning activity use case scenarios.

#### What is xCaliper ?

What is labeled and described in more detail throughout this proposed, is "xCaliper".  xCaliper is a service level solution that acts as a "proxy" or trasducer service, between a data collection/generator or "producer" end-point and a data consumption or "consumption" end-point, to transform any producer xAPI data collection API generated events to an IMS Caliper standardard JSON form stream received by the consumption end-point. This enables producer end-points to generate events in either using the xAPI statement and profile/"recipe" API, or the more semantic learning activity modeled IMS Caliper API, with all events serialized into a common IMS Caliper standard JSON formed stream transmitted to a target consumer end-point (i.e. LRS, RT analytics service etc).  Since the IMS Caliper event model and JSON structure is sufficiently equipped to support both xAPI's flexible non-standard events and Caliper's more semantically structured standardized events, this enables producers to utilize the data generation/collection API that is most effectice for their application while transmitting to consumption end-points as single IMS Caliper standard JSON stream for further processing.  Consumption end-points therefore remain opaque to and therefore need not specialize handling for any differing producer API utilized.  Applications that currently use the xAPI API to collect/generate data because it is sufficient for the end-to-end transmission needed, can continue to do so with the xCaliper service enabled if the consumption end-point is required also to handle IMS Caliper collected/generated events as well.  Also, if an xAPI producer application has additional requirements that warrant the usage of the more semantically enabled and extensible, learning activity modeled IMS Caliper API, this can easily be introduced by the producer without concern over having to transform respective JSON transmission streams to aiign with the consumption end-point targeted. 

#### Expected Market Impact for xCaliper

xCaliper is intended to be an initial step, to first and foremost, provide a unified "bridge" for xAPI to be supported as seamlessly and lossless as possible within the Caliper overarching event model and JSON stream framework.  In essense providing a compatibility for xAPI within Caliper so that producers and consumers can rely on the Caliper superset and IMS certified standard as the unifying framework going forward, especially when both xAPI and Caliper implementation support may ofen be required simultaneously to support the requiremens.  Furthermore, this initial xCaliper implementation and ongong enhancements will enable both the Caliper and xAPI development communities to work more closely together to synchronize on individual efforts to make incremental improvements to educational analytics activity modeling and instrumentation as well as to work together on joint initiatives to create a single, consistent treatment for further expansion of learning activity event modeling via common vocabulary and ontology within the context of the Caliper Event Model framework.

During the initial cycle of xCaliper service implementaiton, the expectation is there will continue to be independent work and adoption in respective Caliper and xAPI communities, but once xCaliper is completed in its initial form there will be a viable solution in the market for the increasing number of analytics implementations that need to support both Caliper and xAPI based data collection within their institution's academic enterprise.  The details of how the respective IMS and xAPI development workgroups and communities will begin to work together needs to be addressed so as to create a more formalized and structured collaborative workgroup effort.

## The xCaliper Technical Solution
**_<<Suggest that we re-orient this section to effectively outline the technical solution proposed to leverage Caliper event model and JSON as the backbone superset to enable a bridge for supporting in parallel xAPI statement and "recipe"/profile generated / collected data transmitted via Caliper JSON to a Caliper supporting end-point >>_**

### xAPI challenges

#### Remapping xAPI verbs to Caliper actions

The absence of firm xAPI governance and curation practices means that anyone can mint a verb for use in an xAPI statement.  That said, Rustici has made an attempt to provide the xAPI community with a registry of xAPI verbs.  Most of the "Tincan" verbs are drawn from the W3C Activity Streams 1.0 specification, a verb set much reduced in size in the re-scoped Activity Streams 2.0 release.  ADL's set of xAPI verbs are also included.  Rustici has itself contributed a robust set of verbs to the registry.  A small set of additional verbs are drawn from a variety of commercial organizations or individuals including Brindleway, HT2 Labs (Curatr), RISC and Andrew Downes among others.  Not all verbs in the Rustici registry focus on learning (e.g., "laughed", "purchased", "ran", "walked"). 

| &nbsp; | Caliper | ADL xAPI | Rustici xAPI | Activity Streams 2.0 | Activity Streams 1.0 | Others |
| :----- | :-------| :------- | :----------- | :------------------- | :------------------- | ----- |
| Total  | 64 | 30 | 46 | 28 | 88 | 19 |

Term duplication across the various verb vocabularies exists which the xCaliper service can leverage to map xAPI verbs to Caliper actions as in the following example:

```
https://w3id.org/xapi/adl/verbs/logged-in  -->  http://purl.imsglobal.org/caliper/actions/LoggedIn
```
  
However, it would be wrong to overplay our current ability to remap large numbers of xAPI verbs to Caliper actions.  We possess no statistics on xAPI Statement verb usage.  Likely, a good number of verbs described in the Rustici Tincan verb registry will never be encountered.  Nevertheless, we anticipate that the xCaliper service will need to process many xAPI verb object representations that have no ready equivalents in Caliper.  In such cases, xAPI verb IRIs will be need to be assigned as values to ```Event.action```.

One example should suffice.  The ADL vocabulary is comprised of 30 verbs.  As of today, we can remap 13 (43.33%) of the ADL verbs to Caliper actions.  Of the 13 verbs we can achieve an exact match on 8 (26.66%) and a near match on 5 (16.66%), such as swapping in "modified" for "updated."  This is not a robust number.

Over time the number of Caliper described actions will increase as existing profiles are revised and new profiles are added to the model.  For instance, the draft Caliper Digital Badges profiles adds a dozen new actions.  As the action footprint grows our ability to map xAPI verbs to Caliper actions will be increase.

Mapping xAPI verbs to Caliper actions will require the establishment and maintenance of data dictionaries.  Defining and publishing equivalencies between verbs and actions as well as Entities and xAPI activity types should be included in the scope of the proposed IMS "Profiles Registry".  Indeed, we should propose to Rustici that they consider retiring their commercial-backed registry in favor of an IMS-sponsored registry.

#### Discerning types

Statement and object typing is limited and map poorly to Caliper.  Unlike Caliper, which distinguishes between `Event` types for descriptive purposes and as an aid to querying (e.g., `AssessmentEvent`, `MessageEvent` etc.), xAPI Statement "types" can be defined optionally by specifying a `context.contextActivities` object typed with an array of one or more "category" object values.  Each category is defined as "an Activity used to categorize the Statement" which in practice is used to link a Statement to a profile such as cmi5 in order to facilitate Statement search and retrieval (see cmi5 9.6.2 [contextActivities](https://github.com/AICC/CMI-5_Spec_Current/blob/quartz/cmi5_spec.md#context_activities).  

Statement "types" can also be inferred from the `object.objectType` property.  Available `objectType` values are limited to "Activity", "Agent", "Group", "SubStatement" and "StatementRef".  If an xAPI `object` is typed as an `Activity` it may include an optional activity `definition` object property that provides additional metadata regarding the activity including a `type` IRI.  This approach results in an unnecessarily awkward if not overly complex JSON representation of an xAPI `object` when compared to a matching Caliper representation, which is decidely more compact and does not require resorting to its optional `extensions` property to reference basic information like a due date:  

JISC xAPI Assignment submitted [recipe](https://github.com/jiscdev/xapi/blob/master/recipes/assignment-submitted.md) `object`

```
"object": {
        "objectType": "Activity",
        "id": "http://moodle.data.alpha.jisc.ac.uk/course/view.php?id=4",
        "definition": {
            "type": "http://adlnet.gov/expapi/activities/assessment",
            "name": {
                "en": "xapiAssignment"
            },
            "description": {
                "en": "xppiAssignmentdescription"
            }
        },
        "extensions": {
            "http://xapi.jisc.ac.uk/dueDate": "2016-02-05T17:59:45.000Z"
        }
    }
```

Caliper Assignment `object` (matching properties only; other available optional properties excluded)

```
{
  "id": "https://example.edu/terms/201601/courses/7/sections/1/assess/1",
  "type": "Assessment",
  "name": "Quiz One",
  "description": "Introductory Quiz",
  "dateToSubmit": "2016-09-28T11:59:59.000Z"
}
```

Second, the decision to infer a Statement type via its `object` leads to a conflation of the `object` of an interaction with the activity itself.  An xAPI Statement `object` is described simultaneously as both "the thing that was acted on" (e.g., an essay)--which aligns with the Caliper notion of an `object`--but also as the activity itself as in "Jeff wrote an essay about hiking."  Activity as a term is used very loosely throughout the xAPI specification and related profiles and recipes.  I find this confusing, especially when in relation to the `object` of a Statement.  This contrasts with Caliper where an `object` is defined narrowly as an `Entity` of a particular type (e.g., `Assessment`, `Message`, `VideoObject` etc.) that, if acted upon by an `actor` at a particular moment in time is termed an `Event`.  Yet for both Caliper and xAPI, an activity can comprise one of more statements or events.

#### Statement Result and Caliper Generatables  
Unlike Caliper, xAPI provides its Statement with a top-level `result` property.  This sort of privileging is understandable given xAPI's SCORM antecedent and roots in corporate training where compliance is a key goal.  Caliper treats it's `Result` as one of a number of "generated" entities that may be produced during an interaction such as grading an assignment.  Other Caliper generated entities such as `Annotation`, `Attempt` and `Response` have no place within the xAPI Statement model outside of `context.extensions`, a property that represents a semantic black hole within the xAPI ecosystem.

#### Statement id handling
There is no firm requirement as is the case with Caliper that a Statement provider MUST set a Statement `id`; it is only a recommendation.  If a Statement is received without an `id` the LRS MUST set it.  If a Statement provider were to emit statement copies sans `id` to multiple endpoints a scenario could arise in which the same Statement could be stamped with different `id` values.

#### Statement timestamp handling
Unlike Caliper Event providers, xAPI Statement providers are not required to provide a `timestamp` value indicating when a statement occurred.  If a Statement does not include a `timestamp` value the receiving LRS must set it.  As in the case of Statements sent without an identifier, should a provider send Statement copies without a timestamp to multiple LRS instances, there is a strong likelihood that each copy will receive a different timestamp.

*Statement context*

TODO: summarize


### xAPI Statement to Caliper Event mappings

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

`object`.  See xAPI-Spec [2.4.4](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#244-object).

`context.registration`.  "When an LRS is an integral part of an LMS, the LMS likely supports the concept of registration. The Experience API applies the concept of registration more broadly. A registration could be considered to be an attempt, a session, or could span multiple Activities. There is no expectation that completing an Activity ends a registration. Nor is a registration necessarily confined to a single Agent.

The Registration is also used when storing documents within the State Resource, e.g. for bookmarking. Normally the same registration is used for requests to both the Statement and State Resources relating to the same learning experience so that all data recorded for the experience is consistent."  See xAPI-Spec [2.4.6.1](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#2461-registration-property).

`context.instructor`.  Instructor associated with the Statement if not already defined as the Statement `actor`. See xAPI-Spec [2.4.6](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#246-context).

`context.team`.  Team associated with the Statement if not already defined as the Statement `actor`.  See xAPI-Spec [2.4.6](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#246-context).

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

See xAPI-Spec [2.4.6.2](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#2462-contextactivities-property).

`context.revision`. The "revision" property MUST only be used if the Statement's Object is an Activity; The "revision" property SHOULD be used to track fixes of minor issues (like a spelling error); The "revision" property SHOULD NOT be used if there is a major change in learning objectives, pedagogy, or assets of an Activity. (Use a new Activity id instead).  See xAPI-Spec [2.4.6](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#246-context).

`context.platform`. See xAPI-Spec [Appendix A](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#appendix-a-example-statements) or a JISC [example](https://github.com/jiscdev/xapi/blob/master/recipes/assignment-submitted.md). 

`context.language`.  RFC 5646 code "representing the language in which the experience being recorded in this Statement (mainly) occurred in, if applicable and known."  See xAPI-Spec [2.4.6](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#246-context).

`context.statement`.  Another Statement to be considered as context for this Statement.  See xAPI-Spec [Statement References](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#statement-references).

```
{
  . . .
  "extensions": {
    "xapi": {
      "context": {
        "statement": {
          "id": "8f87ccde-bb56-4c2e-ab83-44982ef22df0",
          "objectType": "StatementRef"
        }
      }
    }
  }
}
```


### Object to Entity mappings

| xAPI | Type | Conformance | Caliper | Type | Conformance | Notes |
| :--- | :--- | :---------- | :------ | :--- | :---------- | :---- |
| `id` | UUID | Required | `Entity.id` | IRI | Required | Remap Activity identifier as urn:uuid\<UUID\>. |
| `objectType` | string | Optional | `Entity.extensions.xapi.objectType` | string | Optional | xAPI: if defined value MUST = "Activity". |
| `definition` | Object | Optional | &nbsp; | &nbsp; | &nbsp; | Caliper: map attributes to various `Entity` properties. |
| `definition.type` | IRI | Recommended | `Entity.type` | Required | If a mapping exists between the type and a Caliper `Entity` type use the Caliper type; otherwise use generic `Entity` type and add `Entity.extensions.xapi.definition.type`. |
| `definition.name` | Language Map | Recommended | `Entity.name` | string | Optional | Caliper: either ignore the language map or store the key (e.g., "en-US") as `Entity.extensions.xapi.definition.name.key.languageTag`. |
| `definition.description` | Language Map | Recommended | `Entity.description` | string | Optional | Caliper: either ignore the language map or store the key (e.g., "en-US") as `Entity.extensions.xapi.definition.description.key.languageTag`. |
| `definition.moreInfo` | IRL | Optional | `Entity.extensions.xapi.definition.moreInfo` | 
| `definition.extensions` | Object | Optional | `Entity.extensions` | Object | Optional | &nbsp; |


#### Notes

`definition.moreInfo`: "Resolves to a document with human-readable information about the Activity, which could include a way to launch the activity."


#### Interaction activities

xAPI includes built-in definitions for interactions or assessment items based in part on the 
SCORM 2004 4th Edition Data Model. "Since 1.0.3, direct references to the SCORM data model have started to be removed, and any associated requirements included directly in this document. . . .  These interaction definitions are simple to use, and consequently limited. It is expected that Communities of Practice requiring richer interactions definitions will do so through the use of Activity types and Activity Definition Extensions."  See xAPI-Spec [Interaction Activities](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#interaction-activities).

Caliper 1.1 does not model individual question types only response types.  However, the following approach could be taken to map xAPI interaction activities to a Caliper `AssessmentItem`.    

Proposed mapping rule

If the xAPI Statement `object` `definition.type` property equals "http://adlnet.gov/expapi/activities/cmi.interaction" then set the Entity type to "AssessmentItem", copy `definition.description.en-US` to `AssessmentItem.description` (given that the langauge map key can vary this may prove tricky), then copy the xAPI `object` `definition to `AssessmentItem.extensions`.

```
{
  "id": "https://example.edu/terms/201601/courses/7/sections/1/assess/1/items/3",
  "type": "AssessmentItem",
  "description": "Which of these prototypes are available at the beta site?",
  "extensions": {
    "xapi": {
      "definition": {
        "description": {
          "en-US": "Which of these prototypes are available at the beta site?"
        },
        "type": "http://adlnet.gov/expapi/activities/cmi.interaction",
        "interactionType": "choice",
        "correctResponsesPattern": [
          "golf[,]tetris"
        ],
        "choices": [{
            "id": "golf",
            "description": {
              "en-US": "Golf Example"
            }
          },
          {
            "id": "facebook",
            "description": {
              "en-US": "Facebook App"
            }
          },
          {
            "id": "tetris",
            "description": {
              "en-US": "Tetris Example"
            }
          },
          {
            "id": "scrabble",
            "description": {
              "en-US": "Scrabble Example"
            }
          }
        ]
      }
    }
  }
}
```

See xAPI-Spec [Appendix C](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#appendix-c-example-definitions-for-activities-of-type-cmiinteraction) for additional `cmi.interaction` type examples.








`definition` interaction activities.

> Traditional e-learning has included structures for interactions or assessments. As a way to allow these practices and structures to extend Experience API's utility, this specification includes built-in definitions for interactions, which borrows from the SCORM 2004 4th Edition Data Model. These definitions are intended to provide a simple and familiar utility for recording interaction data. Since 1.0.3, direct references to the SCORM data model have started to be removed, and any associated requirements included directly in this document.



  See xAPI-Spec [interaction Activities](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#interaction-activities)






Other mappings











| Caliper | Type | xAPI | Type | Notes |
| :------ | :----| :--- | :--- | :---- | 
| DigitalResource.isPartOf | Entity | context.contextActivities | contextActivities | Maps roughly to contextActivity typed as "Parent" |


#### JISC Statement
{
  "version": "1.0.0",
  "id": "c3e2b586-8923-412c-8259-5210ceb79a2f",
  "timestamp": "2015-12-11T10:19:49.000Z",
  "actor": {
    "objectType": "Agent",
    "name": "test1 test1",
    "account": {
      "homePage": "https://jisc.blackboard.com",
      "name": "test1"
    }
  },
  "verb": {
    "id": "https://brindlewaye.com/xAPITerms/verbs/loggedin",
    "display": {
      "en": "logged in to"
    }
  },
  "object": {
    "objectType": "Activity",
    "id": "https://jisc.blackboard.com/webapps/login/",
    "definition": {
      "type": "http://activitystrea.ms/schema/1.0/application",
      "name": {
        "en": "Blackboard (https://jisc.blackboard.com)"
      },
      "description": {
        "en": "Blackboard (https://jisc.blackboard.com)"
      },
      "extensions": {
        "http://xapi.jisc.ac.uk/applicationType": "http://id.tincanapi.com/activitytype/lms"
      }
    }
  },
  "context": {
    "platform": "Blackboard",
    "extensions": {
      "http://xapi.jisc.ac.uk/plugin/info": {
        "https://jisc.blackboard.com": "9.1.201410",
        "https://jisc.blackboard.com/webapps/bbc-xAPI-BBLEARN/": "1.0.0"
      },
      "http://xapi.jisc.ac.uk/sessionId": "32456891",
      "http://xapi.jisc.ac.uk/recipeVersion": "1.0",
      "http://id.tincanapi.com/extension/ip-address": "10.3.3.48"
    }
  }
}



  







### xAPI nuggets of goodness

1.  Voiding statements
https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#232-voiding

2.  Interaction activities (AssessmentItem types)
https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#interaction-activities

3.  `Statement.stored`

4.  `Statement.authority`

5.  `Statement.attachments`.  Maybe.

6.  StatementRef / `context.statement`

7.  [Signed statements](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#26-signed-statements).  Statements can include a digital signature in the form of a JSON web signature (JWS) as an attachment.

9. `context.instructor`.  Consider adding an instructors [] array to `Course` and `CourseSection`. 
