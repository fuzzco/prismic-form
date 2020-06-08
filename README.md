Quick form builder for Prismic and Vue.

## Instructions

1. **Prismic**
    1. Create a new repeatable type called `form`.
    1. Add a slice zone to the new `form` type and paste the options in `form-type.json`.
1. **Source**
    1. Copy the contents of the `prismic` directory into your `components` directory.
1. **Usage**
    1. Build out the form itself as an instance of the `form` type.
    1. Include a `Content Relationship` field where you want the form to render. Link to the form from the previous step here.
    1. After [registering the `prismic-form` component](https://vuejs.org/v2/guide/components-registration.html) (`prismic/Form.vue`), pass the form to the component in the `form` prop:
       `<prismic-form :form="your_form"/>`
       The component will handle fetching the data and building the form.
