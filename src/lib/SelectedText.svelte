<script>
    import { onMount } from "svelte";

    let selection = ``;
    let text = `This is an example sentence.`;

    function surroundSelection() {
        var span = document.createElement("div");
        span.className = "marked";

        if (window.getSelection) {
            var sel = window.getSelection();
            if (sel.rangeCount) {
                var range = sel.getRangeAt(0).cloneRange();
                range.surroundContents(span);
                sel.removeAllRanges();
                sel.addRange(range);
            }
        }
    }
</script>

<p>
    Je onderzoekt een gegeven (well-defined) probleem met behulp van aangereikte
    methodieken voor desk- en fieldresearch. Je betrekt de doelgroep bij dit
    proces.  Je formuleert een praktische oplossing vanuit de
    onderzoeksresultaten en de behoefte/belangen/waarden van de doelgroep.
</p>

<button on:click={surroundSelection}>Mark</button>

<style>
    * {
        font-size: 2rem;
    }
</style>
