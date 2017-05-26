





## xAPI deficiencies

1. xAPI Statement semantics as regards Statement types are limited and lack sufficient precision.  Unlike Caliper, which distinguishes between `Event` types in order to aid querying, an xAPI Statement type is inferred from the `objectType` property of the `object`.  Available `objectType` values are limited to: `Activity`, `Agent`, `Group`, `SubStatement` and `StatementRef`.  
2. An xAPI `object` is described as "the thing that was acted on" (e.g., an essay).  This aligns with Caliper's notion of an `object`.  Yet an xAPI representation of digital content as the `object` of an interaction is typed as an `Activity` as in "Jeff wrote an essay about hiking."  This sort of definitional imprecision . . . .     
