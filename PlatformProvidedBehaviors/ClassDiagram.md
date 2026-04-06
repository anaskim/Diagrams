```mermaid
classDiagram
    class ScriptWrappable {
        <<abstract>>
    }

    class ElementBehavior {
        <<new, abstract>>
        +BehaviorName() const char*
        +SetElementInternals(ElementInternals*)
        +HandleActivation(Event&) bool
        +DefaultAriaRole() String
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
        +BehaviorName() const char*
        +IsEffectivelyDisabled() bool
        +HandleActivation(Event&) bool
        +DefaultAriaRole() String
    }

    class HTMLElement {
        +attachInternals(options) ElementInternals ⚡
        +DefaultEventHandler(Event&) void ⚡
        +MatchesDefaultPseudoClass() bool ⚡
        +SupportsFocus() FocusableState ⚡
        +DefaultTabIndex() int ⚡
    }

    class ElementInternals {
        +behaviors: FrozenArray~ElementBehavior~ 🆕
        +FindBehavior~T~() T* 🆕
    }

    class HTMLFormElement {
        +PrepareForSubmission(Event*, Element*) void ⚡
        +ScheduleFormSubmission(Event*, Element*) void ⚡
        +FindDefaultButton() Element* ⚡
    }

    class FormSubmission {
        +Create(form, attrs, event, submitter) FormSubmission* ⚡
    }

    ScriptWrappable <|-- ElementBehavior
    ElementBehavior <|-- HTMLSubmitButtonBehavior
    HTMLElement "1" -- "0..1" ElementInternals : owns
    ElementInternals "1" *-- "0..*" ElementBehavior
    HTMLFormElement ..> FormSubmission : creates
    HTMLSubmitButtonBehavior ..> HTMLFormElement : triggers submission

    style ElementBehavior fill: #90EE90, stroke: #228B22, color: #000000
    style HTMLSubmitButtonBehavior fill: #90EE90, stroke: #228B22, color: #000000
```

*🆕 = new member, ⚡ = modified method, green fill = new class*
