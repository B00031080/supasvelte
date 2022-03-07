<script>
    // @ts-nocheck
    import { supabase } from "../supabaseClient";

    let apiResult = null;

    import {
        TextInput,
        TextArea,
        Button,
        FormGroup,
        Form,
        InlineNotification,
    } from "carbon-components-svelte";
    import { createForm } from "svelte-forms-lib";

    const {
        form,
        errors,
        handleChange,
        handleSubmit,
        isSubmitting,
        handleReset,
    } = createForm({
        initialValues: { name: "", email: "", message: "" },
        onSubmit: async (values) => {
            try {
                var result = await supabase.from("contact").insert(values);

                if (result.data != null) {
                    apiResult = true;
                } else {
                    apiResult = false;
                }
            } catch (ex) {
                apiResult = false;
            }

            handleReset();
        },
    });
</script>

<div class="page">
    <h2 class="text-green-700 text-2xl">Contact Us</h2>
    <p>
        Enter the details to get in touch with us. You can remove your email at
        any time after this.
    </p>

    {#if apiResult != null}

        {#if apiResult == true}
        <InlineNotification
            lowContrast
            kind="success"
            title="Success:"
            subtitle="Your message has been received"
        />
        {:else}
        <InlineNotification lowContrast kind="error"
            title="Error:"
            subtitle="An internal server error occurred."
        />

        {/if}    
    {/if}

    <br /><br />

    <Form on:submit={handleSubmit}>
        <FormGroup>
            <TextInput
                labelText="Name"
                name="name"
                on:change={handleChange}
                bind:value={$form.name}
            />
        </FormGroup>

        <FormGroup>
            <TextInput
                labelText="Email"
                name="email"
                type="email"
                on:change={handleChange}
                bind:value={$form.email}
            />
        </FormGroup>

        <FormGroup>
            <TextArea
                labelText="Message"
                name="message"
                type="textarea"
                on:change={handleChange}
                bind:value={$form.message}
            />
        </FormGroup>

        <Button type="submit" disabled={$isSubmitting}>Submit</Button>
    </Form>
</div>
