```mermaid
classDiagram
    class ScriptWrappable {
        <<abstract>>
    }

    class ElementBehavior {
        <<new, abstract>>
        +BehaviorName() const char*
        +SetElementInternals(ElementInternals*)
        +GetElementInternals() ElementInternals*
        +HandleActivation(Event&) bool
        +DefaultAriaRole() ax::mojom::blink::Role
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
        +DefaultAriaRole() ax::mojom::blink::Role
        +IsActivatedSubmit() bool
        +SetActivatedSubmit(bool)
    }

    class Element {
        +SubmitBehavior() HTMLSubmitButtonBehavior* 🆕
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
        +BehaviorBasedDefaultRole() ax::mojom::blink::Role 🆕
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
    Element <|-- HTMLElement
    HTMLElement "1" -- "0..1" ElementInternals : owns
    ElementInternals "1" *-- "0..*" ElementBehavior
    HTMLFormElement ..> FormSubmission : creates
    HTMLSubmitButtonBehavior ..> HTMLFormElement : triggers submission
    Element ..> HTMLSubmitButtonBehavior : SubmitBehavior()

    style ElementBehavior fill: #90EE90, stroke: #228B22, color: #000000
    style HTMLSubmitButtonBehavior fill: #90EE90, stroke: #228B22, color: #000000
```
*🆕 = new member, ⚡ = modified method, green fill = new class*
