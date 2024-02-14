---
layout: post
title: Bypassing Formik Yup Validation
authorId: simon_timms
tags:
  - javascript
  - formik
  - yup
categories:
  - development
date: 2024-02-14
---

I've been using Formik with Yup validations for a while and it really a pretty good experience. The integration is tight and really all one needs to do is to define the validation schema and then pass it to Formik via the `validationSchema`. The validation is then run on every change to the form and the errors are displayed.

```javascript
const schema = Yup.object().shape({
    restricted: Yup.bool().required("Required"),
    payer: Yup.string().required("Required"),
    feeSchedule: Yup.string().required("Required")
    ...
  });

  return (
    <Formik validationSchema={schema}
    onSubmit={handleSubmit}
    initialValues={{
        restricted: false,
        payer: "",
        feeSchedule: ""
    }}>
        ...
        <FormikSubmitButton label="Save"/>
    </Formik>
  )

```

But today's challenge was that we had a request to show validation errors but still submit the form. I have a custom submit button control I use which hooks into the Formik context and pulls out the `submitForm` and `isValid` properties. It then disables the button if the form isn't valid. This means that I can have a really consistent look and feel to my forms' submit buttons. 

It looks a bit like this

```javascript
const FormikSubmitButton = (props) => {
  const { label, sx, disabled, ...restProps } = props;

  const { submitForm, isValid } = useFormikContext();

  const isDisabled = (!isValid || disabled);
  
  return (
    <>
      <Button
        disabled={isDisabled}
        onClick={submitForm}
        variant="contained"
        color="primaryAction"
        sx={sx}
        {...restProps}
      >
        {label}
      </Button>
    </>
  );
};
```

So the button disabled itself if the form isn't valid. Now instead we need to bypass this so I passed in a new parameter `allowInvalid` which would allow the button to be clicked even if the form wasn't valid. 

```javascript
const FormikSubmitButton = (props) => {
  const { label, sx, disabled, allowInvalid, ...restProps } = props;

  const { submitForm, isValid } = useFormikContext();

  const isDisabled = (!isValid || disabled) && !allowInvalid;
  
  return (
    <>
      <Button
        disabled={isDisabled}
        onClick={submitForm}
        variant="contained"
        color="primaryAction"
        sx={sx}
        {...restProps}
      >
        {label}
      </Button>
    </>
  );
};
```

This fixes the button disabling itself but it doens't resolve the issue of the form not submitting. The `submitForm` function from Formik will not submit the form if it is invalid. To get around this I had to call the `handleSubmit` function from the form and then manually set the form to be submitted. This is enough of a change that I wanted a whole separate component for it. 

```javascript
const FormikValidationlessSubmitButton = (props) => {
  const { label, sx, disabled, onSubmit, ...restProps } = props;

  const { setSubmitting, values } = useFormikContext();

  const handleSubmit(){
    setSubmitting(true);
    if(onSubmit){
      onSubmit(values);
    }
    setSubmitting(false);
  }

  return (
    <>
      <Button
        disabled={disabled}
        onClick={handleSubmit}
        variant="contained"
        color="primaryAction"
        sx={sx}
        {...restProps}
      >
        {label}
      </Button>
    </>
  );
};
```

This component can then be used in place of the `FormikSubmitButton` when you want to bypass validation. 

```javascript
<Formik validationSchema={schema}
    onSubmit={handleSubmit}
    initialValues={{
        restricted: false,
        payer: "",
        feeSchedule: ""
    }}>
        ...
        <FormikValidationlessSubmitButton onSubmit={handleSubmit} label="Save"/>
    </Formik>
```