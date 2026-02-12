# ===================================================================
# Coding Best Practices as Code: Complete Specification v1.0
# This document defines all coding standards, principles, and practices
# that can be systematically applied and validated by automated systems.
# ===================================================================

[environment]
target_languages = ["rust", "python", "javascript", "typescript", "java", "c++", "go"]
validation_tools = ["clippy", "eslint", "pylint", "sonarqube", "cppcheck"]
static_analysis_engines = ["tree-sitter", "ast-grep", "semgrep"]
complexity_metrics = ["cyclomatic", "cognitive", "halstead", "maintainability_index"]

# ===================================================================
# FOUNDATIONAL PRINCIPLES
# Core software engineering principles that guide all other practices
# ===================================================================

[principles.solid]

DEFINE_PRINCIPLE(single_responsibility) {
  [manifest] {
    scope = ["class", "function", "module", "component"],
    enforcement = "mandatory",
    validation_strategy = "complexity_analysis"
  }
  rule: "Each unit of code should have exactly one reason to change"
  metrics = {
    max_public_methods = 7,
    max_function_lines = 50,
    max_cyclomatic_complexity = 10,
    max_cognitive_complexity = 15
  }
  violation_patterns = ["god_objects", "swiss_army_knife_functions", "data_classes_with_behavior"]
  auto_fix = ["SplitClass", "ExtractMethod", "ExtractInterface"]
}

DEFINE_PRINCIPLE(open_closed) {
  [manifest] {
    scope = ["class", "interface", "trait", "module"],
    enforcement = "recommended",
    validation_strategy = "dependency_analysis"
  }
  rule: "Software entities should be open for extension, closed for modification"
  preferred_patterns = ["strategy_pattern", "decorator_pattern", "plugin_architecture", "trait_objects"]
  violation_patterns = ["massive_switch_statements", "if_else_chains", "hardcoded_implementations"]
  auto_fix = ["ExtractStrategy", "CreateInterface", "ImplementVisitor"]
}

DEFINE_PRINCIPLE(liskov_substitution) {
  [manifest] {
    scope = ["inheritance", "trait_impl", "interface_impl"],
    enforcement = "mandatory",
    validation_strategy = "contract_testing"
  }
  rule: "Derived classes must be substitutable for their base classes"
  validation_tests = ["precondition_weakening", "postcondition_strengthening", "invariant_preservation"]
  violation_patterns = ["strengthened_preconditions", "weakened_postconditions", "exception_throwing_overrides"]
  auto_fix = ["GenerateContractTests", "RefactorHierarchy", "ExtractCommonInterface"]
}

DEFINE_PRINCIPLE(interface_segregation) {
  [manifest] {
    scope = ["interface", "trait", "abstract_class"],
    enforcement = "recommended",
    validation_strategy = "dependency_analysis"
  }
  rule: "Clients should not be forced to depend on interfaces they don't use"
  metrics = {
    max_interface_methods = 5,
    max_interface_complexity = 20
  }
  violation_patterns = ["fat_interfaces", "kitchen_sink_traits", "marker_interfaces_with_methods"]
  auto_fix = ["SplitInterface", "ExtractRoleInterface", "CreateAdapterInterface"]
}

DEFINE_PRINCIPLE(dependency_inversion) {
  [manifest] {
    scope = ["constructor", "method_parameters", "field_declarations"],
    enforcement = "recommended",
    validation_strategy = "dependency_analysis"
  }
  rule: "High-level modules should not depend on low-level modules. Both should depend on abstractions"
  preferred_patterns = ["dependency_injection", "inversion_of_control", "abstract_factory"]
  violation_patterns = ["new_in_constructor", "concrete_coupling", "hardcoded_dependencies"]
  auto_fix = ["ExtractInterface", "InjectDependency", "CreateFactory"]
}

[principles.dry]

DEFINE_PRINCIPLE(dont_repeat_yourself) {
  [manifest] {
    scope = ["function", "class", "module", "configuration"],
    enforcement = "mandatory",
    validation_strategy = "semantic_analysis"
  }
  rule: "Every piece of knowledge must have a single, unambiguous, authoritative representation"
  detection_thresholds = {
    code_similarity = 0.8,
    minimum_lines = 3,
    string_repetition = 2,
    magic_number_repetition = 2
  }
  violation_patterns = ["copy_paste_programming", "magic_numbers", "hardcoded_strings", "duplicated_logic"]
  auto_fix = ["ExtractMethod", "ExtractConstant", "ExtractConfiguration", "CreateUtilityFunction"]
}

[principles.kiss]

DEFINE_PRINCIPLE(keep_it_simple_stupid) {
  [manifest] {
    scope = ["function", "class", "algorithm"],
    enforcement = "recommended",
    validation_strategy = "complexity_analysis"
  }
  rule: "Simplicity should be a key goal in design, and unnecessary complexity should be avoided"
  metrics = {
    max_nesting_depth = 4,
    max_parameters = 5,
    max_return_statements = 3,
    max_boolean_expressions = 3
  }
  violation_patterns = ["over_engineering", "premature_optimization", "unnecessary_abstraction"]
  auto_fix = ["SimplifyConditions", "ExtractMethod", "InlineVariable", "RemoveDeadCode"]
}

[principles.yagni]

DEFINE_PRINCIPLE(you_arent_gonna_need_it) {
  [manifest] {
    scope = ["feature", "method", "class"],
    enforcement = "recommended",
    validation_strategy = "usage_analysis"
  }
  rule: "Don't implement functionality until you actually need it"
  violation_patterns = ["unused_methods", "over_generalization", "future_proofing", "speculative_features"]
  auto_fix = ["RemoveUnusedCode", "SimplifyGeneric", "InlineOverAbstraction"]
}

# ===================================================================
# CODE STRUCTURE & ORGANIZATION
# Principles governing how code should be structured and organized
# ===================================================================

[structure.naming]

DEFINE_PRACTICE(consistent_naming) {
  [manifest] {
    scope = ["variable", "function", "class", "module"],
    enforcement = "mandatory",
    validation_strategy = "pattern_matching"
  }
  conventions = {
    rust = { functions = "snake_case", types = "PascalCase", constants = "SCREAMING_SNAKE_CASE" },
    python = { functions = "snake_case", classes = "PascalCase", constants = "SCREAMING_SNAKE_CASE" },
    javascript = { functions = "camelCase", classes = "PascalCase", constants = "SCREAMING_SNAKE_CASE" },
    java = { methods = "camelCase", classes = "PascalCase", constants = "SCREAMING_SNAKE_CASE" }
  }
  violation_patterns = ["inconsistent_casing", "abbreviations", "hungarian_notation"]
  auto_fix = ["RenameSymbol", "ExpandAbbreviation", "StandardizeCasing"]
}

DEFINE_PRACTICE(meaningful_names) {
  [manifest] {
    scope = ["variable", "function", "class"],
    enforcement = "mandatory",
    validation_strategy = "semantic_analysis"
  }
  rule: "Names should reveal intent and be pronounceable"
  metrics = {
    min_name_length = 3,
    max_name_length = 50,
    max_abbreviations = 0
  }
  violation_patterns = ["single_letter_variables", "meaningless_names", "mental_mapping_names"]
  exceptions = ["loop_counters", "mathematical_formulas", "well_known_abbreviations"]
  auto_fix = ["SuggestMeaningfulName", "ExpandAbbreviation", "AddContextualPrefix"]
}

[structure.functions]

DEFINE_PRACTICE(function_size) {
  [manifest] {
    scope = ["function", "method"],
    enforcement = "mandatory",
    validation_strategy = "line_counting"
  }
  rule: "Functions should be small and focused"
  metrics = {
    max_lines = 50,
    max_statements = 20,
    max_parameters = 5,
    max_nesting_depth = 4
  }
  violation_patterns = ["long_functions", "parameter_lists", "deep_nesting"]
  auto_fix = ["ExtractMethod", "ParameterObject", "GuardClauses", "ReduceNesting"]
}

DEFINE_PRACTICE(pure_functions) {
  [manifest] {
    scope = ["function", "method"],
    enforcement = "recommended",
    validation_strategy = "side_effect_analysis"
  }
  rule: "Prefer pure functions that don't modify state or cause side effects"
  violation_patterns = ["global_state_mutation", "hidden_side_effects", "non_deterministic_functions"]
  auto_fix = ["ExtractSideEffects", "MakeImmutable", "ReturnNewState"]
}

[structure.classes]

DEFINE_PRACTICE(class_cohesion) {
  [manifest] {
    scope = ["class", "struct"],
    enforcement = "mandatory",
    validation_strategy = "cohesion_analysis"
  }
  rule: "Classes should have high cohesion - all methods should work together toward a common purpose"
  metrics = {
    min_cohesion_score = 0.7,
    max_public_methods = 10,
    max_fields = 10
  }
  violation_patterns = ["god_objects", "data_classes", "utility_classes"]
  auto_fix = ["SplitClass", "ExtractInterface", "MoveMethod"]
}

DEFINE_PRACTICE(encapsulation) {
  [manifest] {
    scope = ["field", "method", "class"],
    enforcement = "mandatory",
    validation_strategy = "visibility_analysis"
  }
  rule: "Hide implementation details and expose only necessary interfaces"
  violation_patterns = ["public_fields", "unnecessary_public_methods", "exposed_internals"]
  auto_fix = ["MakePrivate", "CreateGetter", "CreateSetter", "ExtractInterface"]
}

# ===================================================================
# ERROR HANDLING & SAFETY
# Practices for robust error handling and safe code
# ===================================================================

[safety.error_handling]

DEFINE_PRACTICE(explicit_error_handling) {
  [manifest] {
    scope = ["function", "method", "async_block"],
    enforcement = "mandatory",
    validation_strategy = "exception_analysis"
  }
  rule: "All potential errors must be explicitly handled or propagated"
  language_specifics = {
    rust = { preferred = "Result<T, E>", forbidden = ["unwrap", "expect", "panic"] },
    java = { preferred = "checked_exceptions", forbidden = ["catch_all", "empty_catch"] },
    python = { preferred = "explicit_exceptions", forbidden = ["bare_except", "pass_in_except"] }
  }
  violation_patterns = ["ignored_exceptions", "generic_catch_all", "swallowed_errors"]
  auto_fix = ["ConvertToResult", "AddSpecificCatch", "LogAndRethrow"]
}

DEFINE_PRACTICE(fail_fast) {
  [manifest] {
    scope = ["function", "constructor", "validation"],
    enforcement = "recommended",
    validation_strategy = "control_flow_analysis"
  }
  rule: "Validate inputs and fail early rather than continuing with invalid state"
  patterns = ["guard_clauses", "precondition_checks", "input_validation"]
  violation_patterns = ["defensive_programming", "silent_failures", "late_validation"]
  auto_fix = ["AddGuardClauses", "ExtractValidation", "ThrowEarly"]
}

DEFINE_PRACTICE(resource_management) {
  [manifest] {
    scope = ["resource_allocation", "cleanup", "disposal"],
    enforcement = "mandatory",
    validation_strategy = "resource_analysis"
  }
  rule: "All acquired resources must be properly released"
  language_specifics = {
    rust = { preferred = "RAII", patterns = ["Drop_trait", "scoped_resources"] },
    java = { preferred = "try_with_resources", patterns = ["AutoCloseable", "finally_blocks"] },
    python = { preferred = "context_managers", patterns = ["with_statement", "__enter__/__exit__"] }
  }
  violation_patterns = ["resource_leaks", "missing_cleanup", "manual_resource_management"]
  auto_fix = ["AddResourceCleanup", "UseContextManager", "ImplementRAII"]
}

# ===================================================================
# PERFORMANCE & EFFICIENCY
# Practices for writing efficient and performant code
# ===================================================================

[performance.algorithms]

DEFINE_PRACTICE(algorithmic_complexity) {
  [manifest] {
    scope = ["function", "method", "loop"],
    enforcement = "recommended",
    validation_strategy = "complexity_analysis"
  }
  rule: "Choose appropriate algorithms and data structures for the problem"
  metrics = {
    max_time_complexity = "O(n^2)",
    max_space_complexity = "O(n)",
    max_nested_loops = 2
  }
  violation_patterns = ["nested_loops", "inefficient_algorithms", "wrong_data_structures"]
  auto_fix = ["SuggestBetterAlgorithm", "OptimizeDataStructure", "CacheResults"]
}

DEFINE_PRACTICE(memory_efficiency) {
  [manifest] {
    scope = ["allocation", "collection", "caching"],
    enforcement = "recommended",
    validation_strategy = "memory_analysis"
  }
  rule: "Use memory efficiently and avoid unnecessary allocations"
  violation_patterns = ["memory_leaks", "excessive_allocations", "large_object_retention"]
  auto_fix = ["ObjectPooling", "LazyInitialization", "WeakReferences"]
}

DEFINE_PRACTICE(premature_optimization) {
  [manifest] {
    scope = ["function", "class", "algorithm"],
    enforcement = "warning",
    validation_strategy = "profiling_analysis"
  }
  rule: "Don't optimize without measuring performance first"
  violation_patterns = ["micro_optimizations", "complex_caching", "premature_parallelization"]
  auto_fix = ["AddProfiler", "SimplifyOptimization", "MeasureFirst"]
}

# ===================================================================
# TESTING & QUALITY ASSURANCE
# Practices for ensuring code quality through testing
# ===================================================================

[testing.coverage]

DEFINE_PRACTICE(test_coverage) {
  [manifest] {
    scope = ["function", "class", "module"],
    enforcement = "mandatory",
    validation_strategy = "coverage_analysis"
  }
  rule: "All code must have appropriate test coverage"
  metrics = {
    min_line_coverage = 80,
    min_branch_coverage = 75,
    min_function_coverage = 90
  }
  violation_patterns = ["untested_code", "dead_code", "unreachable_branches"]
  auto_fix = ["GenerateTests", "RemoveDeadCode", "AddTestCases"]
}

DEFINE_PRACTICE(test_quality) {
  [manifest] {
    scope = ["test_function", "test_class"],
    enforcement = "mandatory",
    validation_strategy = "test_analysis"
  }
  rule: "Tests should be reliable, maintainable, and fast"
  metrics = {
    max_test_runtime = "5s",
    max_test_lines = 20,
    max_assertions_per_test = 5
  }
  patterns = ["arrange_act_assert", "given_when_then", "single_concept_per_test"]
  violation_patterns = ["flaky_tests", "slow_tests", "unclear_tests"]
  auto_fix = ["SplitTest", "MockDependencies", "ClarifyAssertions"]
}

DEFINE_PRACTICE(test_naming) {
  [manifest] {
    scope = ["test_function", "test_class"],
    enforcement = "mandatory",
    validation_strategy = "naming_analysis"
  }
  rule: "Test names should clearly describe what is being tested"
  patterns = ["should_when_given", "methodName_stateUnderTest_expectedBehavior"]
  violation_patterns = ["generic_test_names", "unclear_test_purpose", "abbreviated_test_names"]
  auto_fix = ["GenerateDescriptiveName", "AddTestDescription", "StandardizeNaming"]
}

# ===================================================================
# SECURITY PRACTICES
# Practices for writing secure code
# ===================================================================

[security.input_validation]

DEFINE_PRACTICE(input_sanitization) {
  [manifest] {
    scope = ["user_input", "api_parameter", "file_input"],
    enforcement = "mandatory",
    validation_strategy = "data_flow_analysis"
  }
  rule: "All external input must be validated and sanitized"
  violation_patterns = ["sql_injection", "xss_vulnerabilities", "path_traversal", "command_injection"]
  auto_fix = ["AddInputValidation", "UsePreparedStatements", "EscapeOutput"]
}

DEFINE_PRACTICE(authentication_authorization) {
  [manifest] {
    scope = ["api_endpoint", "method", "resource_access"],
    enforcement = "mandatory",
    validation_strategy = "access_control_analysis"
  }
  rule: "All protected resources must have proper authentication and authorization"
  violation_patterns = ["missing_authentication", "insufficient_authorization", "privilege_escalation"]
  auto_fix = ["AddAuthCheck", "ImplementRBAC", "ValidatePermissions"]
}

DEFINE_PRACTICE(secrets_management) {
  [manifest] {
    scope = ["configuration", "environment", "code"],
    enforcement = "mandatory",
    validation_strategy = "secret_scanning"
  }
  rule: "No secrets should be hardcoded or stored in plain text"
  violation_patterns = ["hardcoded_passwords", "api_keys_in_code", "plain_text_secrets"]
  auto_fix = ["ExtractToConfig", "UseSecretManager", "EncryptSecrets"]
}

# ===================================================================
# DOCUMENTATION & MAINTAINABILITY
# Practices for maintaining readable and documented code
# ===================================================================

[documentation.comments]

DEFINE_PRACTICE(meaningful_comments) {
  [manifest] {
    scope = ["function", "class", "complex_logic"],
    enforcement = "recommended",
    validation_strategy = "documentation_analysis"
  }
  rule: "Comments should explain why, not what"
  patterns = ["api_documentation", "business_logic_explanation", "edge_case_documentation"]
  violation_patterns = ["obvious_comments", "outdated_comments", "commented_code"]
  auto_fix = ["RemoveObviousComments", "UpdateStaleComments", "RemoveCommentedCode"]
}

DEFINE_PRACTICE(api_documentation) {
  [manifest] {
    scope = ["public_method", "public_class", "public_interface"],
    enforcement = "mandatory",
    validation_strategy = "documentation_coverage"
  }
  rule: "All public APIs must have comprehensive documentation"
  required_sections = ["description", "parameters", "return_value", "exceptions", "examples"]
  violation_patterns = ["missing_documentation", "incomplete_documentation", "outdated_documentation"]
  auto_fix = ["GenerateDocumentation", "AddMissingSection", "UpdateDocumentation"]
}

[documentation.code_readability]

DEFINE_PRACTICE(readable_code) {
  [manifest] {
    scope = ["function", "class", "expression"],
    enforcement = "recommended",
    validation_strategy = "readability_analysis"
  }
  rule: "Code should be self-documenting and easy to understand"
  metrics = {
    max_complexity = 10,
    max_nesting = 3,
    max_line_length = 120
  }
  violation_patterns = ["complex_expressions", "unclear_logic", "long_lines"]
  auto_fix = ["ExtractVariable", "SimplifyExpression", "BreakLongLines"]
}

# ===================================================================
# CONCURRENCY & PARALLELISM
# Practices for safe concurrent programming
# ===================================================================

[concurrency.thread_safety]

DEFINE_PRACTICE(thread_safety) {
  [manifest] {
    scope = ["shared_state", "mutable_data", "concurrent_access"],
    enforcement = "mandatory",
    validation_strategy = "concurrency_analysis"
  }
  rule: "All shared mutable state must be properly synchronized"
  violation_patterns = ["race_conditions", "deadlocks", "data_races", "unsynchronized_access"]
  auto_fix = ["AddSynchronization", "UseImmutableData", "ImplementLockFree"]
}

DEFINE_PRACTICE(async_patterns) {
  [manifest] {
    scope = ["async_function", "promise", "future"],
    enforcement = "recommended",
    validation_strategy = "async_analysis"
  }
  rule: "Use appropriate async patterns and avoid blocking operations"
  violation_patterns = ["blocking_in_async", "callback_hell", "promise_anti_patterns"]
  auto_fix = ["ConvertToAsync", "FlattenCallbacks", "UseProperAsyncPattern"]
}

# ===================================================================
# LANGUAGE-SPECIFIC PRACTICES
# Practices specific to particular programming languages
# ===================================================================

[language_specific.rust]

DEFINE_PRACTICE(ownership_borrowing) {
  [manifest] {
    scope = ["variable", "function_parameter", "return_value"],
    enforcement = "mandatory",
    validation_strategy = "borrow_checker"
  }
  rule: "Follow Rust ownership and borrowing rules efficiently"
  violation_patterns = ["unnecessary_clones", "lifetime_issues", "borrow_checker_fights", "rc_refcell_overuse"]
  auto_fix = ["OptimizeBorrowing", "AddLifetimeAnnotations", "UseReferences", "ConvertToSharedOwnership"]
  best_practices = [
    "prefer_borrowing_over_cloning",
    "use_cow_for_conditional_ownership", 
    "leverage_zero_cost_abstractions",
    "avoid_string_cloning_in_loops"
  ]
}

DEFINE_PRACTICE(error_handling_rust) {
  [manifest] {
    scope = ["function", "method"],
    enforcement = "mandatory",
    validation_strategy = "result_analysis"
  }
  rule: "Use Result<T, E> for error handling, avoid panic in library code"
  violation_patterns = ["unwrap_in_library", "expect_without_reason", "panic_in_library", "unwrap_or_default_misuse"]
  auto_fix = ["ConvertToResult", "AddErrorContext", "HandleGracefully", "UseQuestionMark"]
  best_practices = [
    "use_thiserror_for_custom_errors",
    "implement_from_for_error_conversion",
    "use_anyhow_for_application_errors",
    "prefer_question_mark_operator"
  ]
}

DEFINE_PRACTICE(memory_safety) {
  [manifest] {
    scope = ["unsafe_block", "raw_pointer", "ffi"],
    enforcement = "mandatory",
    validation_strategy = "unsafe_analysis"
  }
  rule: "Minimize unsafe code and document safety invariants"
  violation_patterns = ["unnecessary_unsafe", "undocumented_safety", "unsafe_aliasing", "dangling_pointers"]
  auto_fix = ["AddSafetyComments", "WrapInSafeAPI", "UseStdAbstractions"]
  best_practices = [
    "document_all_safety_invariants",
    "minimize_unsafe_surface_area",
    "use_miri_for_undefined_behavior_detection",
    "prefer_safe_abstractions"
  ]
}

DEFINE_PRACTICE(trait_design) {
  [manifest] {
    scope = ["trait_definition", "trait_implementation"],
    enforcement = "recommended",
    validation_strategy = "trait_analysis"
  }
  rule: "Design coherent, composable traits following Rust conventions"
  violation_patterns = ["marker_traits_with_methods", "overly_generic_traits", "blanket_impl_conflicts"]
  auto_fix = ["SplitTrait", "AddWhereClause", "RefactorBlanketImpl"]
  best_practices = [
    "follow_orphan_rule",
    "use_associated_types_over_generics",
    "implement_common_traits_consistently",
    "prefer_composition_over_inheritance"
  ]
}

DEFINE_PRACTICE(performance_rust) {
  [manifest] {
    scope = ["hot_path", "allocation", "iteration"],
    enforcement = "recommended",
    validation_strategy = "performance_analysis"
  }
  rule: "Write efficient Rust code leveraging zero-cost abstractions"
  violation_patterns = ["allocating_in_loops", "string_concatenation_in_loops", "vec_push_without_capacity"]
  auto_fix = ["PreallocateCapacity", "UseStringBuilder", "OptimizeIterators"]
  best_practices = [
    "use_iterators_over_loops",
    "preallocate_collections",
    "prefer_stack_allocation",
    "use_cow_for_conditional_cloning",
    "leverage_simd_when_appropriate"
  ]
}

DEFINE_PRACTICE(concurrency_rust) {
  [manifest] {
    scope = ["async_function", "thread_spawn", "channel_usage"],
    enforcement = "recommended",
    validation_strategy = "concurrency_analysis"
  }
  rule: "Use Rust's concurrency primitives safely and efficiently"
  violation_patterns = ["blocking_in_async", "arc_mutex_overuse", "channel_misuse", "thread_pool_abuse"]
  auto_fix = ["ConvertToAsync", "OptimizeConcurrency", "UseProperPrimitives"]
  best_practices = [
    "prefer_message_passing_over_shared_state",
    "use_tokio_for_async_runtime",
    "leverage_rayon_for_data_parallelism",
    "avoid_async_in_destructors"
  ]
}

DEFINE_PRACTICE(module_organization) {
  [manifest] {
    scope = ["module", "crate", "visibility"],
    enforcement = "recommended",
    validation_strategy = "module_analysis"
  }
  rule: "Organize code into logical, well-encapsulated modules"
  violation_patterns = ["flat_module_structure", "circular_dependencies", "pub_everything"]
  auto_fix = ["CreateSubmodules", "ExtractModule", "RefineVisibility"]
  best_practices = [
    "use_lib_rs_for_library_interface",
    "group_related_functionality",
    "minimize_public_api_surface",
    "use_re_exports_judiciously"
  ]
}

[language_specific.csharp]

DEFINE_PRACTICE(nullable_reference_types) {
  [manifest] {
    scope = ["reference_type", "method_parameter", "property"],
    enforcement = "mandatory",
    validation_strategy = "nullability_analysis"
  }
  rule: "Use nullable reference types and handle null cases explicitly"
  violation_patterns = ["null_reference_exceptions", "unchecked_null_access", "nullable_disabled"]
  auto_fix = ["EnableNullableContext", "AddNullChecks", "UseNullableAnnotations"]
  best_practices = [
    "enable_nullable_reference_types_globally",
    "use_null_conditional_operators",
    "prefer_null_object_pattern",
    "validate_parameters_early"
  ]
}

DEFINE_PRACTICE(async_await_patterns) {
  [manifest] {
    scope = ["async_method", "task_usage", "await_expression"],
    enforcement = "mandatory",
    validation_strategy = "async_analysis"
  }
  rule: "Use async/await properly and avoid common pitfalls"
  violation_patterns = ["sync_over_async", "task_wait_deadlock", "async_void_non_event", "missing_configure_await"]
  auto_fix = ["AddConfigureAwait", "ConvertToAsync", "FixDeadlock", "UseTaskInsteadOfVoid"]
  best_practices = [
    "use_configure_await_false_in_libraries",
    "prefer_task_over_async_void",
    "use_cancellation_tokens",
    "avoid_blocking_on_async_code",
    "use_task_when_all_for_concurrency"
  ]
}

DEFINE_PRACTICE(memory_management) {
  [manifest] {
    scope = ["disposable_resource", "finalizer", "weak_reference"],
    enforcement = "mandatory",
    validation_strategy = "resource_analysis"
  }
  rule: "Properly manage memory and implement IDisposable pattern"
  violation_patterns = ["missing_dispose", "finalizer_without_dispose", "disposing_twice", "large_object_retention"]
  auto_fix = ["ImplementDisposable", "AddUsingStatement", "FixDisposePattern"]
  best_practices = [
    "implement_dispose_pattern_correctly",
    "use_using_statements_for_disposables",
    "avoid_finalizers_unless_necessary",
    "use_weak_references_for_caches",
    "pool_large_objects"
  ]
}

DEFINE_PRACTICE(exception_handling) {
  [manifest] {
    scope = ["try_catch", "exception_throwing", "custom_exception"],
    enforcement = "mandatory",
    validation_strategy = "exception_analysis"
  }
  rule: "Handle exceptions appropriately and create meaningful custom exceptions"
  violation_patterns = ["catching_all_exceptions", "throwing_system_exceptions", "losing_stack_traces", "exceptions_for_control_flow"]
  auto_fix = ["SpecificExceptionCatch", "CreateCustomException", "PreserveStackTrace", "RefactorControlFlow"]
  best_practices = [
    "catch_specific_exceptions_only",
    "create_domain_specific_exceptions",
    "use_inner_exceptions_for_context",
    "fail_fast_for_programming_errors",
    "log_exceptions_at_boundaries"
  ]
}

DEFINE_PRACTICE(linq_optimization) {
  [manifest] {
    scope = ["linq_query", "enumerable_methods", "collection_operations"],
    enforcement = "recommended",
    validation_strategy = "performance_analysis"
  }
  rule: "Use LINQ efficiently and avoid performance pitfalls"
  violation_patterns = ["multiple_enumeration", "inefficient_queries", "linq_in_loops", "unnecessary_to_list"]
  auto_fix = ["CacheEnumerable", "OptimizeQuery", "ExtractFromLoop", "RemoveUnnecessaryToList"]
  best_practices = [
    "use_ienumerable_over_concrete_collections",
    "prefer_foreach_over_linq_for_side_effects",
    "cache_expensive_enumerables",
    "use_any_instead_of_count_for_existence",
    "leverage_parallellinq_for_cpu_intensive_work"
  ]
}

DEFINE_PRACTICE(dependency_injection) {
  [manifest] {
    scope = ["constructor", "service_registration", "lifetime_management"],
    enforcement = "recommended",
    validation_strategy = "di_analysis"
  }
  rule: "Use dependency injection properly with appropriate lifetimes"
  violation_patterns = ["service_locator_antipattern", "captive_dependencies", "circular_dependencies", "inappropriate_lifetimes"]
  auto_fix = ["RefactorToConstructorInjection", "FixLifetimes", "BreakCircularDependency"]
  best_practices = [
    "prefer_constructor_injection",
    "register_services_by_interface",
    "use_appropriate_service_lifetimes",
    "avoid_service_locator_pattern",
    "validate_dependency_graph"
  ]
}

DEFINE_PRACTICE(type_design) {
  [manifest] {
    scope = ["class_design", "struct_design", "interface_design"],
    enforcement = "recommended",
    validation_strategy = "type_analysis"
  }
  rule: "Design types following .NET conventions and best practices"
  violation_patterns = ["mutable_structs", "reference_types_as_structs", "fat_interfaces", "god_classes"]
  auto_fix = ["ConvertToClass", "SplitInterface", "ExtractResponsibility", "MakeImmutable"]
  best_practices = [
    "prefer_classes_for_complex_types",
    "use_structs_for_small_immutable_values",
    "implement_equality_consistently",
    "override_tostring_meaningfully",
    "use_records_for_data_containers"
  ]
}

DEFINE_PRACTICE(security_practices) {
  [manifest] {
    scope = ["data_access", "authentication", "cryptography"],
    enforcement = "mandatory",
    validation_strategy = "security_analysis"
  }
  rule: "Follow .NET security best practices"
  violation_patterns = ["sql_injection", "weak_cryptography", "insecure_deserialization", "hardcoded_secrets"]
  auto_fix = ["UseParameterizedQueries", "UseStrongCrypto", "SecureDeserialization", "ExtractSecrets"]
  best_practices = [
    "use_parameterized_queries_always",
    "implement_proper_authentication",
    "use_https_for_all_communications",
    "validate_all_inputs",
    "use_secure_random_generators",
    "implement_proper_authorization"
  ]
}

DEFINE_PRACTICE(performance_optimization) {
  [manifest] {
    scope = ["collection_usage", "string_operations", "boxing_unboxing"],
    enforcement = "recommended",
    validation_strategy = "performance_analysis"
  }
  rule: "Write performant C# code avoiding common performance pitfalls"
  violation_patterns = ["string_concatenation_in_loops", "boxing_in_loops", "incorrect_collection_choice", "linq_abuse"]
  auto_fix = ["UseStringBuilder", "AvoidBoxing", "OptimizeCollections", "RefactorLinq"]
  best_practices = [
    "use_stringbuilder_for_multiple_concatenations",
    "choose_appropriate_collection_types",
    "avoid_boxing_in_hot_paths",
    "use_span_and_memory_for_performance",
    "leverage_object_pooling_for_expensive_objects",
    "profile_before_optimizing"
  ]
}

[language_specific.python]

DEFINE_PRACTICE(pythonic_code) {
  [manifest] {
    scope = ["function", "class", "expression"],
    enforcement = "recommended",
    validation_strategy = "idiom_analysis"
  }
  rule: "Write idiomatic Python code following PEP 8 and Python conventions"
  patterns = ["list_comprehensions", "context_managers", "duck_typing", "generators"]
  violation_patterns = ["c_style_loops", "unnecessary_lambdas", "mutable_defaults"]
  auto_fix = ["ConvertToListComp", "UseContextManager", "FixMutableDefault"]
}

[language_specific.javascript]

DEFINE_PRACTICE(modern_javascript) {
  [manifest] {
    scope = ["function", "variable", "class"],
    enforcement = "recommended",
    validation_strategy = "es_version_analysis"
  }
  rule: "Use modern JavaScript features and avoid deprecated patterns"
  patterns = ["arrow_functions", "destructuring", "template_literals", "async_await"]
  violation_patterns = ["var_declarations", "function_constructors", "string_concatenation"]
  auto_fix = ["ConvertToArrowFunction", "UseDestructuring", "UseTemplateLiterals"]
}

# ===================================================================
# INTEGRATION WITH BUILD PIPELINE
# How these practices integrate with the build system
# ===================================================================

[integration.validation_stages]

DEFINE_STAGE(static_analysis) {
  practices = ["naming", "complexity", "documentation", "security_scanning"]
  tools = ["clippy", "eslint", "pylint", "sonarqube"]
  failure_strategy = "SequentialDebug"
}

DEFINE_STAGE(architectural_validation) {
  practices = ["solid_principles", "dry_violations", "coupling_analysis"]
  tools = ["dependency_analysis", "architecture_tests"]
  failure_strategy = "Warning"
}

DEFINE_STAGE(quality_gates) {
  practices = ["test_coverage", "performance_benchmarks", "security_tests"]
  tools = ["coverage_tools", "performance_profilers", "security_scanners"]
  failure_strategy = "Halt"
}

[integration.actors]

DEFINE_ACTOR(PracticeBee) {
  responsibilities = ["apply_coding_practices", "validate_compliance", "suggest_improvements"]
  input_types = ["source_code", "ast", "dependency_graph"]
  output_types = ["validated_code", "violation_report", "improvement_suggestions"]
}

DEFINE_ACTOR(RefactoringBee) {
  responsibilities = ["apply_refactoring_patterns", "fix_violations", "improve_code_quality"]
  input_types = ["violation_report", "source_code", "refactoring_rules"]
  output_types = ["refactored_code", "refactoring_report"]
}

DEFINE_ACTOR(QualityAnalyzer) {
  responsibilities = ["measure_code_quality", "track_technical_debt", "generate_reports"]
  input_types = ["source_code", "test_results", "metrics"]
  output_types = ["quality_report", "debt_analysis", "trend_analysis"]
}

# ===================================================================
# ENFORCEMENT CONFIGURATION
# How practices are enforced and validated
# ===================================================================

[enforcement.severity_levels]

DEFINE_SEVERITY(mandatory) {
  action = "block_build"
  notification = "immediate"
  auto_fix = "attempt"
}

DEFINE_SEVERITY(recommended) {
  action = "warn"
  notification = "summary"
  auto_fix = "suggest"
}

DEFINE_SEVERITY(warning) {
  action = "log"
  notification = "report"
  auto_fix = "none"
}

[enforcement.exemptions]

DEFINE_EXEMPTION(legacy_code) {
  scope = ["legacy_modules", "third_party_code"]
  practices = ["naming_conventions", "documentation_requirements"]
  expiration = "2025-12-31"
}

DEFINE_EXEMPTION(performance_critical) {
  scope = ["hot_paths", "performance_critical_functions"]
  practices = ["complexity_limits", "memory_efficiency"]
  justification_required = true
}

# ===================================================================
# METRICS AND MEASUREMENT
# How to measure adherence to practices
# ===================================================================

[metrics.quality_gates]

DEFINE_METRIC(code_quality_score) {
  formula = "weighted_average(practice_compliance_scores)"
  weights = {
    solid_principles = 0.25,
    dry_violations = 0.20,
    test_coverage = 0.20,
    security_practices = 0.15,
    documentation = 0.10,
    performance = 0.10
  }
  min_acceptable = 0.8
}

DEFINE_METRIC(technical_debt_ratio) {
  formula = "total_debt_hours / total_development_hours"
  max_acceptable = 0.05
  measurement_interval = "weekly"
}

DEFINE_METRIC(maintainability_index) {
  formula = "171 - 5.2 * ln(average_cyclomatic_complexity) - 0.23 * average_cyclomatic_complexity - 16.2 * ln(lines_of_code)"
  min_acceptable = 85
  scope = ["module", "class", "function"]
}

# ===================================================================
# CONTINUOUS IMPROVEMENT
# How practices evolve and improve over time
# ===================================================================

[improvement.feedback_loops]

DEFINE_FEEDBACK_LOOP(practice_effectiveness) {
  metrics = ["defect_reduction", "development_velocity", "code_review_time"]
  evaluation_period = "monthly"
  adjustment_strategy = "data_driven"
}

DEFINE_FEEDBACK_LOOP(developer_satisfaction) {
  metrics = ["practice_adoption_rate", "developer_surveys", "resistance_patterns"]
  evaluation_period = "quarterly"
  adjustment_strategy = "collaborative"
}

[improvement.evolution]

DEFINE_EVOLUTION_STRATEGY(practice_refinement) {
  triggers = ["new_language_features", "industry_best_practices", "team_feedback"]
  process = ["proposal", "trial", "measurement", "adoption"]
  rollback_strategy = "gradual_revert"
}

# ===================================================================
# REPORTING AND ANALYTICS
# How to report on practice adherence and code quality
# ===================================================================

[reporting.dashboards]

DEFINE_DASHBOARD(code_quality_overview) {
  widgets = [
    "quality_score_trend",
    "practice_compliance_matrix",
    "top_violations",
    "improvement_recommendations"
  ]
  refresh_interval = "daily"
  audience = ["development_team", "tech_leads", "management"]
}

DEFINE_DASHBOARD(technical_debt_tracking) {
  widgets = [
    "debt_trend",
    "debt_by_category",
    "paydown_progress",
    "risk_assessment"
  ]
  refresh_interval = "weekly"
  audience = ["tech_leads", "architects", "management"]
}

[reporting.notifications]

DEFINE_NOTIFICATION(quality_gate_failure) {
  trigger = "quality_score < min_acceptable"
  recipients = ["commit_author", "code_reviewers", "tech_lead"]
  escalation = "manager_after_24h"
}

DEFINE_NOTIFICATION(practice_improvement_opportunity) {
  trigger = "repeated_violations OR new_best_practice_available"
  recipients = ["development_team"]
  frequency = "weekly_digest"
}

# ===================================================================
# CUSTOMIZATION AND EXTENSIBILITY
# How teams can customize and extend practices
# ===================================================================

[customization.team_overrides]

DEFINE_OVERRIDE_CAPABILITY(practice_customization) {
  allowed_modifications = ["severity_levels", "thresholds", "exemptions"]
  approval_required = "tech_lead"
  documentation_required = true
}

DEFINE_OVERRIDE_CAPABILITY(language_extensions) {
  allowed_additions = ["new_practices", "language_specific_rules", "custom_validators"]
  approval_required = "architecture_committee"
  testing_required = true
}

[customization.plugins]

DEFINE_PLUGIN_INTERFACE(custom_validators) {
  input_schema = "source_code_ast"
  output_schema = "violation_report"
  integration_points = ["static_analysis_stage", "pre_commit_hooks"]
}

DEFINE_PLUGIN_INTERFACE(custom_fixers) {
  input_schema = "violation_report + source_code"
  output_schema = "fixed_code + change_summary"
  integration_points = ["auto_fix_stage", "ide_integrations"]
}

# ===================================================================
# TOOL INTEGRATION
# How practices integrate with development tools
# ===================================================================

[tools.ide_integration]

DEFINE_INTEGRATION(real_time_validation) {
  supported_ides = ["vscode", "intellij", "vim", "emacs"]
  features = ["live_linting", "quick_fixes", "practice_suggestions"]
  performance_requirements = ["< 100ms response", "< 50MB memory"]
}

DEFINE_INTEGRATION(code_completion) {
  features = ["practice_aware_suggestions", "pattern_completion", "anti_pattern_warnings"]
  ML_models = ["code_pattern_recognition", "best_practice_recommendation"]
}

[tools.ci_cd_integration]

DEFINE_INTEGRATION(build_pipeline) {
  stages = ["pre_commit", "build", "test", "deploy"]
  gates = ["quality_gates", "practice_compliance", "security_checks"]
  reporting = ["build_reports", "quality_trends", "violation_summaries"]
}

DEFINE_INTEGRATION(deployment_gates) {
  criteria = ["quality_score_threshold", "security_compliance", "performance_benchmarks"]
