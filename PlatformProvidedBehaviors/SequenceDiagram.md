```mermaid
sequenceDiagram
    participant User
    participant CustomElement
    participant HTMLElement as HTMLElement::DefaultEventHandler
    participant EI as ElementInternals
    participant Behavior as HTMLSubmitButtonBehavior
    participant Form as HTMLFormElement
    participant FS as FormSubmission

    User->>CustomElement: click
    CustomElement->>HTMLElement: DOMActivate event
    HTMLElement->>EI: FindBehavior<HTMLSubmitButtonBehavior>()
    EI-->>HTMLElement: HTMLSubmitButtonBehavior* (or nullptr)
    alt Has submit behavior && !IsEffectivelyDisabled()
        HTMLElement->>Behavior: HandleActivation(event)
        Behavior->>EI: Form()
        EI-->>Behavior: HTMLFormElement*
        Behavior->>Form: PrepareForSubmission(event, element, behavior)
        Form->>Form: ScheduleFormSubmission(event, element, behavior)
        Form->>FS: Create(form, attrs, event, element, behavior)
        Note over FS: Branches on behavior: applies<br/>overrides, builds FormData, request
        FS-->>Form: FormSubmission*
        Note over Form: CSP checks,<br/>UseCounters, navigation
        Form-->>User: Form submitted
    end
```
