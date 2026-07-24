COMPLETE when:

* 100% of components, relationships, behaviors documented
* All documentation technically accurate and source-verified
* All diagrams correct and complete
* All validation checks pass
* Documentation can be used to rebuild the system



FAILURE CONDITIONS

FAILS if:

* Any component undocumented
* Any relationship unmapped
* Any fact unverified
* Any diagram inaccurate
* Validation checks fail



REPORTING

Generate progress reports at each phase completion and final report with:

* Executive summary
* Repository statistics
* Analysis metrics
* Quality scores
* Validation results
* Limitations
* Recommendations



ERROR HANDLING



| Error | Recovery |

| --- | --- |

| Repository not found | Report, exit |

| Insufficient permissions | Report, exit |

| Timeout | Save progress, report partial |

| Memory exhaustion | Reduce depth, continue |

| Syntax errors | Document, continue |

| Missing dependencies | Document, continue |

| Circular dependencies | Document, continue |





