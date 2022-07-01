# 🧪 Forms

We use react-hook-form to create forms.

- [React Hook Form](https://react-hook-form.com/)

The following code excerpt demonstrates a basic usage example:

```javascript
import * as React from "react";
import { useForm } from "react-hook-form";

type FormData = {
  firstName: string;
  lastName: string;
};

export default function App() {
  const { register, setValue, handleSubmit, formState: { errors } } = useForm<FormData>();
  const onSubmit = handleSubmit(data => console.log(data));
  // firstName and lastName will have correct type

  return (
    <form onSubmit={onSubmit}>
      <label>First Name</label>
      <input {...register("firstName")} />
      <label>Last Name</label>
      <input {...register("lastName")} />
      <button
        type="button"
        onClick={() => {
          setValue("lastName", "luo"); // ✅
          setValue("firstName", true); // ❌: true is not string
          errors.bill; // ❌: property bill does not exist
        }}
      >
        SetValue
      </button>
    </form>
  );
}
```

# Kiwi form components

You can use @yousign/kiwi components with react-hook-form. Dedicated components for forms are already in @yousign/kiwi

## Form validation

To do form validation you can use react-hook-form and yup together.

- [yup](https://github.com/jquense/yup)
- [SchemaValidation](https://react-hook-form.com/get-started#SchemaValidation)

## Accessibility
- [Forms accessibility] https://www.w3.org/WAI/tutorials/forms/