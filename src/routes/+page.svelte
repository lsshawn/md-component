<script>
  import Rectangle from '../lib/components/Rectangle.svelte';
  import { onMount } from 'svelte';

  import { marked} from 'marked'

  let parsedContent = [];
  const markdown = `
# Title

<Rectangle color="red"/>
<Rectangle color="green"/>
`;

  onMount(() => {
    parseMarkdown();
  });

  function parseMarkdown() {
    const lines = markdown.split('\n');
    parsedContent = lines.map(line => {
      if (line.trim().startsWith('<Rectangle')) {
        const colorMatch = line.match(/color="(.+?)"/);
        const color = colorMatch ? colorMatch[1] : 'blue';
        return { type: 'rectangle', color };
      }
      return { type: 'text', content: line };
    });
  }
</script>

{#each parsedContent as item}
  {#if item.type === 'text'}
    {@html marked(item.content)}
  {:else if item.type === 'rectangle'}
    <Rectangle color={item.color}/>
  {/if}
{/each}

