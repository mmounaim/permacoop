<script>
  import { goto } from '@sapper/app';
  import { _ } from 'svelte-i18n';
  import Breadcrumb from '../../../../components/Breadcrumb.svelte';
  import { post } from '../../../../utils/axios';
  import Form from './_Form.svelte';
  import { errorNormalizer } from '../../../../normalizer/errors';
  import ServerErrors from '../../../../components/ServerErrors.svelte';
  import H4Title from '../../../../components/H4Title.svelte';

  let title = $_('human_resources.leaves.requests.add.title');
  let loading = false;
  let errors = [];

  const onSave = async (e) => {
    try {
      loading = true;
      await post('leave-requests', e.detail);
      goto('/human_resources/leaves/requests');
    } catch (e) {
      errors = errorNormalizer(e);
    } finally {
      loading = false;
    }
  };
</script>

<svelte:head>
  <title>{title} - {$_('app')}</title>
</svelte:head>

<Breadcrumb
  items="{[
    { title: $_('human_resources.breadcrumb') }, 
    { title: $_('human_resources.leaves.title'), path: '/human_resources/leaves' }, 
    { title: $_('human_resources.leaves.requests.title'), path: '/human_resources/leaves/requests' }, 
    { title }
  ]}" />
<ServerErrors errors="{errors}" />
<H4Title title="{title}" />
<Form on:save="{onSave}" loading="{loading}" />
