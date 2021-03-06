# Formik Validators

## Basic usage

```js
import validator, { required, minLength } from 'formik-validators'

const MyForm = withFormik({
  handleSubmit,
  validate: validator({
    phoneNumber: [
      required('errors.required'),
      minLength(10)('errors.minLength')
    ]
  })
})(InnerForm)
```

## Custom the translate function
By default, the translate function would return the same string (expect a translation term) that you give it
`translateFn = (term, params) => term`.

If you are not going to support i18n, feel free to use a plain text instead.

On the other side, you can absolutely create your own translate function.

Example: using I18n.t as a translate function

```js
// validator.js
import I18n from 'react-native-i18n'
import validator, { setTranslateFn } from 'formik-validators'

setTranslateFn((term, params) => I18n.t(term, params))

export default validator
```

```js
// ../../forms/index.js
export { default as validator } from './validator'
export * from './forms.components'
```

```js
// myModule.form.js
import { validators, TextInput } from '../../forms'
import { required, minLength, getFieldProps } from 'formik-validators'
const InnerForm = props => {
  const fieldProps = getFieldProps(props)
  return (
    <View>
      <TextInput
        {...fieldProps}
        name={'userName'}
        placeholder={'Username'}
      />
    </View>
  )
}
```

## Add new validation rules

`formik-validators` comes with some basics validation rules. Although I'm continually adding more rules, it's still far to be enough for building a real world app.

The library make it easy for adding your custom rules as long as they returns a `RuleFn` type

Example: add a new rule to check the exact length

```js
const exactLength = (length: number, errorKey: string): RuleFn => ({
  value, values, props
}) => {
  if (isNil(value)) return
  if (value.length !== length) {
    return t(errorKey, { length })
  }
}
```
