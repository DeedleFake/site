<script>
    import Accordion from "@/components/Accordion.svelte";
    import CodeBlock from "@/components/CodeBlock.svelte";
    import CodeTabs from "@/components/CodeTabs.svelte";

    const responses = [
        {
            code: 204,
            body: `null`,
        },
        {
            code: 400,
            body: `
                {
                  "code": 400,
                  "message": "An error occurred while submitting the form.",
                  "data": {
                    "password": {
                      "code": "validation_required",
                      "message": "Missing required value."
                    }
                  }
                }
            `,
        },
    ];

    let responseTab = responses[0].code;
</script>

<Accordion single title={"Confirm password reset"}>
    <div class="content m-b-sm">
        <p>Confirms a password reset request and sets a new admin password.</p>
    </div>

    <CodeTabs
        js={`
            import PocketBase from 'pocketbase';

            const pb = new PocketBase('http://127.0.0.1:8090');

            ...

            // change password
            await pb.admins.confirmPasswordReset('TOKEN', '1234567890', '1234567890');

            // reauthenticate
            await pb.admins.authWithPassword("test@example.com", "1234567890")
        `}
        dart={`
            import 'package:pocketbase/pocketbase.dart';

            final pb = PocketBase('http://127.0.0.1:8090');

            ...

            // change password
            await pb.admins.confirmPasswordReset('TOKEN', '1234567890', '1234567890');

            // reauthenticate
            await pb.admins.authWithPassword("test@example.com", "1234567890")
        `}
    />

    <div class="api-route alert alert-success">
        <strong class="label label-primary">POST</strong>
        <div class="content">/api/admins/confirm-password-reset</div>
    </div>

    <div class="section-title">Body Parameters</div>
    <table class="table-compact table-border">
        <thead>
            <tr>
                <th>Param</th>
                <th>Type</th>
                <th width="50%">Description</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>
                    <div class="inline-flex">
                        <span class="label label-success">Required</span>
                        <span class="txt">token</span>
                    </div>
                </td>
                <td>
                    <span class="label">String</span>
                </td>
                <td>The token from the password reset request email.</td>
            </tr>
            <tr>
                <td>
                    <div class="inline-flex">
                        <span class="label label-success">Required</span>
                        <span class="txt">password</span>
                    </div>
                </td>
                <td>
                    <span class="label">String</span>
                </td>
                <td>The new admin password to set.</td>
            </tr>
            <tr>
                <td>
                    <div class="inline-flex">
                        <span class="label label-success">Required</span>
                        <span class="txt">passwordConfirm</span>
                    </div>
                </td>
                <td>
                    <span class="label">String</span>
                </td>
                <td>New admin password confirmation.</td>
            </tr>
        </tbody>
    </table>
    <small class="block txt-hint m-t-10 m-b-base">
        Body parameters could be sent as <em>JSON</em> or
        <em>multipart/form-data</em>.
    </small>

    <div class="section-title">Responses</div>
    <div class="tabs">
        <div class="tabs-header compact left">
            {#each responses as response (response.code)}
                <button
                    class="tab-item"
                    class:active={responseTab === response.code}
                    on:click={() => (responseTab = response.code)}
                >
                    {response.code}
                </button>
            {/each}
        </div>
        <div class="tabs-content">
            {#each responses as response (response.code)}
                <div class="tab-item" class:active={responseTab === response.code}>
                    <CodeBlock content={response.body} />
                </div>
            {/each}
        </div>
    </div>
</Accordion>
