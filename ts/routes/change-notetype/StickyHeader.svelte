<!--
Copyright: Ankitects Pty Ltd and contributors
License: GNU AGPL, version 3 or later; http://www.gnu.org/licenses/agpl.html
-->
<script lang="ts">
    import * as tr from "@generated/ftl";

    import Badge from "$lib/components/Badge.svelte";
    import Col from "$lib/components/Col.svelte";
    import Container from "$lib/components/Container.svelte";
    import Icon from "$lib/components/Icon.svelte";
    import { exclamationIcon } from "$lib/components/icons";
    import Row from "$lib/components/Row.svelte";
    import StickyContainer from "$lib/components/StickyContainer.svelte";

    import Alert from "./Alert.svelte";
    import type { ChangeNotetypeState } from "./lib";
    import { MapContext } from "./lib";

    export let state: ChangeNotetypeState;
    export let ctx: MapContext;

    $: info = state.info;

    const heading: string =
        ctx === MapContext.Field
            ? tr.changeNotetypeFields()
            : tr.changeNotetypeTemplates();

    $: unused =
        $info.isCloze && ctx === MapContext.Template ? [] : $info.unusedItems(ctx);
</script>

<StickyContainer
    --sticky-top={ctx === MapContext.Template ? "-1px" : "0"}
    --sticky-border="var(--border)"
    --sticky-borders="0px 0 1px"
>
    <h1>
        {heading}
        {#if unused.length > 0}
            <Badge iconSize={80}>
                <Icon icon={exclamationIcon} />
            </Badge>
        {/if}
    </h1>

    {#if unused.length > 0}
        <Alert {unused} {ctx} />
    {/if}

    {#if $info.templates}
        <Container --gutter-inline="0.5rem" --gutter-block="0.2rem">
            <Row --cols={2}>
                <Col --col-size={1}><b>{tr.changeNotetypeCurrent()}</b></Col>
                <Col --col-size={1}><b>{tr.changeNotetypeNew()}</b></Col>
            </Row>
        </Container>
    {/if}
</StickyContainer>

<style lang="scss">
    h1 {
        padding-top: 0.5em;
    }
</style>
