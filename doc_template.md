# {{@TITLE}}


{{@DESCRIPTION}}.

---

Namespace: `{{@NAMESPACE=='\\'?'\\':rtrim(@NAMESPACE,'\\')}}` <br>
File location: `lib/{{@FILENAME}}.php`

<check if="{{!empty(@WARNING_THIS_IS_A_DRAFT)}}">
<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This function is currently not documented; only its argument list is available.</p></div>
</check>

<check if="{{@ABSTRACT}}">
<true>
## ~~Instanciation~~
### {{@CLASSNAME}} is an abstract class
</true>
<false>
## Instanciation

**Return class instance**

<check if="{{@EXTENDS == 'Prefab'}}">
<true>
```php
${{strtolower(@CLASSNAME).' = '.@NAMESPACE.(substr(@NAMESPACE,-1)=='\\'?'':'\\').@CLASSNAME}}::instance();
```

The {{@CLASSNAME}} class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.
</true>
<false>
```php
<check if="{{empty(@CONSTRUCTOR)}}">
<true>
${{strtolower(@CLASSNAME)}} = new {{@CLASSNAME}} ();
</true>
<false>
${{strtolower(@CLASSNAME)}} = new {{@CLASSNAME}} ( {{@CONSTRUCTOR.signature_params}} );
</false>
</check>
```
<check if="{{!empty(@EXTENDS)}}">
The {{@CLASSNAME}} class extends the [{{@EXTENDS}}]({{@EXTENDS_URL}}) class.
</check>
<check if="{{!empty(@CONSTRUCTOR)}}">
Please refer to the [__construct]({{@CONSTRUCTOR.fragment_id}}) method for details.
</check>
</false>
</check>
</false>
</check>

## Methods

<repeat group="{{ @METHODS }}" value="{{ @METHOD }}">

### {{ @METHOD.name }}


**{{ @METHOD.description }}**

``` php
{{@METHOD.signature_full}} 
```

This function allows you to {{ lcfirst(@METHOD.description) }} 

Example:

``` php
<check if="{{@METHOD.name!='__construct'}}">
<true>
<check if="!empty({{@METHOD.example}})">
<true>{{@METHOD.example}} </true>
<false>
echo ${{strtolower(@CLASSNAME)}}->{{@METHOD.name}}({{@METHOD.signature_params}}) // displays '@TODO'
</false>
</check>
</true>
<false>
${{strtolower(@CLASSNAME)}} = new {{@CLASSNAME}} ( {{@METHOD.signature_params}} )
</false>
</check>
```
</repeat>
