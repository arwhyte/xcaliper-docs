





### xAPI challenges

#### Discerning types

Statement and object typing is limited and map poorly to Caliper.  Unlike Caliper, which distinguishes between `Event` types for descriptive purposes and as an aid to querying (e.g., `AssessmentEvent`, `MessageEvent` etc.), xAPI Statement "types" can be defined optionally by specifying a `context.contextActivities` object typed with an array of one or more "category" object values.  Each category is defined as "an Activity used to categorize the Statement" which in practice is used to link a Statement to a profile such as cmi5 in order to facilitate Statement search and retrieval (see cmi5 9.6.2 [contextActivities](https://github.com/AICC/CMI-5_Spec_Current/blob/quartz/cmi5_spec.md#context_activities).  

Statement "types" can also be inferred from the `object.objectType` property.  Available `objectType` values are limited to "Activity", "Agent", "Group", "SubStatement" and "StatementRef".  If an xAPI `object` is typed as an `Activity` it may include an optional activity `definition` object property that provides additional metadata regarding the activity including a `type` IRI.  This approach results in an unnecessarily awkward if not overly complex JSON representation of an xAPI `object` when compared to a matching Caliper representation, which is decidely more compact and does not require resorting to its optional `extensions` property to reference basic information like a due date:  

JISC xAPI Assignment submitted [receipe](https://github.com/jiscdev/xapi/blob/master/recipes/assignment-submitted.md) `object`

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

| xAPI | Type | Caliper | Type | Notes |
| :--- | :--- | :------ | :--- | :---- |
| `id` | UUID | `id` | UUID as a URN | Remap `Statement.id` as urn:uuid\<UUID\>. |
| `actor` | Agent \| Group | `actor` | Agent | &nbsp; |
| `verb` | Object | `action` | Term | Remap to `Event.action` and/or adjust Caliper to express an action as either an Object, string IRI or string term.  We can alias the JSON-LD @language keyword in order to map the xAPI `display` property. | 
| `result` | Object | &nbsp; | &nbsp; | Remap to `Event.generated`. |
| `context.group` | Group | `membership` | Membership | xAPI: "Team that the Statement relates to, if not included as the Actor of the Statement."  This relationship should be mappable to `Event.membership` although xAPI does not appear to model an individual member's role or status. |
| `context.language` | String per RFC 5646 | &nbsp; | &nbsp; | No equivalent in Caliper. |
| `context.platform` | String | `edApp` | SoftwareApplication | Even if we coerce the edApp to its IRI the mapping remains weak as there is no xAPI requirement that the value be expressed as an identifier.  See xAPI-Spec [Appendix A](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#appendix-a-example-statements) or a JISC [example](https://github.com/jiscdev/xapi/blob/master/recipes/assignment-submitted.md). |
| &nbsp; | &nbsp; | `group` | Organization | &nbsp; |
| `context.statement` | Statement Reference Object | &nbsp; | &nbsp; | Another Statement considered as context for this Statement.  No equivalent currently in Caliper. |
| `timestamp` | DateTime | `eventTime` | DateTime | Remap if optional `timestamp` is provided by provider or set it (add an extension property that indicates that the consumer set the `eventTime`. |
| `stored` | DateTime | &nbsp; | &nbsp; | xAPI: set by LRS.  No equivalent currently in Caliper. |
| `authority` | Object | &nbsp; | &nbsp; | xAPI: optional.  No equivalent currently in Caliper. |
| `version` | String | &nbsp; | &nbsp; | xAPI: not recommended.  Caliper Events are versioned via reference to the JSON-LD @context and the `Envelope.dataVersion` property. | 
| `attachments` | Ordered array of Attachment Objects | &nbsp; | &nbsp; | xAPI: optional.  Headers for Attachments to the Statement.  No equivalent currently in Caliper. |

Other mappings

| Caliper | Type | xAPI | Type | Notes |
| :------ | :----| :--- | :--- | :---- | 
| DigitalResource.isPartOf | Entity | context.contextActivities | contextActivities | Maps roughly to contextActivity typed as "Parent" |



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