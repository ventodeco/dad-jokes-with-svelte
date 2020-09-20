<script lang="ts">
    import { Form, FormGroup, FormText, Input, Label, Col, Row, Button } from 'sveltestrap';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    let term = "";

    $: canSubmit = term.length > 0;

    function onSearch(e) {
        e.preventDefault();
        if(canSubmit){
            dispatch('search', term);
            term = "";
        }
    }
</script>

<style>
    :global(.search) {
        margin-top: 30px;
        width: 100%;
    }

    :global(button:disabled) {
        cursor: not-allowed;
    }

    @media(max-width: 700px){
        :global(.search){
            margin-top: 0;
        }
    }
</style>

<Form on:submit={onSearch}>
    <Row>
        <Col md="8" xs="12">
            <FormGroup>
                <Label for="dadJokesSearch">Search</Label>
                <Input type="text" id="dadJokesSearch" placeholder="Search For Your Favorite Dad Jokes" bind:value={term} />
            </FormGroup>
        </Col>

        <Col md="4" xs="12">
            <Button type="submit" disabled={!canSubmit} color="primary" class="search">Search</Button>
        </Col>
    </Row>
</Form>