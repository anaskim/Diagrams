```mermaid
classDiagram
    class ScriptWrappable {
        <<abstract>>
    }

    class PlatformBehavior {
        <<new, abstract>>
        +OnAttached(ElementInternals*)
        +OnDetached()
        +HandleActivation(Event&) bool
        +GetDefaultARIARole() String
    }

    class HTMLSubmitButtonBehavior {
        <<new>>
        +disabled: bool
        +form: HTMLFormElement (readonly)
        +formAction: String
        +formEnctype: String
        +formMethod: String
        +formNoValidate: bool
        +formTarget: String
        +labels: NodeList (readonly)
        +name: String
        +value: String
        +IsEffectivelyDisabled() bool
        +HandleActivation(Event&) bool
        +GetDefaultARIARole() String
    }

    class ElementInternals {
        +behaviors: ObservableArray~PlatformBehavior~ 🆕
        +FindBehavior~T~() T* 🆕
        -EnsureBehaviors() ObservableArray& 🆕
    }

    class HTMLElement {
        +attachInternals(options) ElementInternals ⚡
        +DefaultEventHandler(Event&) void ⚡
    }

    class HTMLFormElement {
        +PrepareForSubmission(Event*, Element*, Behavior*) void ⚡
        +ScheduleFormSubmission(Event*, Element*, Behavior*) void ⚡
        +ScheduleFormSubmissionInternal(Event*, FormSubmission*) void 🆕
        +FindDefaultButton() Element* ⚡
    }

    class FormSubmission {
        +Create(form, attrs, event, element, behavior) FormSubmission* ⚡
        -CreateInternal(form, attrs, event, ...) FormSubmission* 🆕
    }

    ScriptWrappable <|-- PlatformBehavior
    PlatformBehavior <|-- HTMLSubmitButtonBehavior
    ElementInternals "1" *-- "0..*" PlatformBehavior : stores
    HTMLElement "1" -- "0..1" ElementInternals : owns
    HTMLSubmitButtonBehavior ..> HTMLFormElement : triggers submission
    HTMLFormElement ..> FormSubmission : creates

    style PlatformBehavior fill: #90EE90, stroke: #228B22, color: #000000
    style HTMLSubmitButtonBehavior fill: #90EE90, stroke: #228B22, color: #000000
```

*🆕 = new member, ⚡ = modified method, green fill = new class*
