```mermaid
classDiagram
    class ScriptWrappable {
        <<abstract>>
    }

    class PlatformBehavior {
        <<new, abstract>>
        +SetElementInternals(ElementInternals*)
        +HandleActivation(Event&) bool
        +GetDefaultARIARole() String
        +IsAttached() bool
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

    class HTMLElement {
        +attachInternals(options) ElementInternals ⚡
        +DefaultEventHandler(Event&) void ⚡
        +MatchesDefaultPseudoClass() bool ⚡
        +SupportsFocus() FocusableState ⚡
        +DefaultTabIndex() int ⚡
    }

    class ElementInternals {
        +behaviors: FrozenArray~PlatformBehavior~ 🆕
        +FindBehavior~T~() T* 🆕
    }

    class HTMLFormElement {
        +PrepareForSubmission(Event*, Element*, Behavior*) void ⚡
        +ScheduleFormSubmission(Event*, Element*, Behavior*) void ⚡
        +FindDefaultButton() Element* ⚡
    }

    class FormSubmission {
        +Create(form, attrs, event, submitter, behavior?) FormSubmission* ⚡
    }

    ScriptWrappable <|-- PlatformBehavior
    PlatformBehavior <|-- HTMLSubmitButtonBehavior
    HTMLElement "1" -- "0..1" ElementInternals : owns
    ElementInternals "1" *-- "0..*" PlatformBehavior
    HTMLFormElement ..> FormSubmission : creates
    HTMLSubmitButtonBehavior ..> HTMLFormElement : triggers submission

    style PlatformBehavior fill: #90EE90, stroke: #228B22, color: #000000
    style HTMLSubmitButtonBehavior fill: #90EE90, stroke: #228B22, color: #000000
```

*🆕 = new member, ⚡ = modified method, green fill = new class*
