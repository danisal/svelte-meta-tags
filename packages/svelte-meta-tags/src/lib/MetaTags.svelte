<script lang="ts">
  import type { MetaTagsProps } from './types';

  export let title: MetaTagsProps['title'] = '';
  export let titleTemplate: MetaTagsProps['titleTemplate'] = '';
  export let robots: MetaTagsProps['robots'] = 'index,follow';
  export let additionalRobotsProps: MetaTagsProps['additionalRobotsProps'] = undefined;
  export let description: MetaTagsProps['description'] = undefined;
  export let mobileAlternate: MetaTagsProps['mobileAlternate'] = undefined;
  export let languageAlternates: MetaTagsProps['languageAlternates'] = undefined;
  export let twitter: MetaTagsProps['twitter'] = undefined;
  export let facebook: MetaTagsProps['facebook'] = undefined;
  export let openGraph: MetaTagsProps['openGraph'] = undefined;
  export let canonical: MetaTagsProps['canonical'] = undefined;
  export let additionalMetaTags: MetaTagsProps['additionalMetaTags'] = undefined;
  export let additionalLinkTags: MetaTagsProps['additionalLinkTags'] = undefined;
  export let keywords: MetaTagsProps['keywords'] = undefined;

  $: updatedTitle = titleTemplate ? (title ? titleTemplate.replace(/%s/g, title) : title) : title;

  let robotsParams = '';
  if (additionalRobotsProps) {
    const {
      nosnippet,
      maxSnippet,
      maxImagePreview,
      maxVideoPreview,
      noarchive,
      noimageindex,
      notranslate,
      unavailableAfter
    } = additionalRobotsProps;

    robotsParams = `${nosnippet ? ',nosnippet' : ''}${maxSnippet ? `,max-snippet:${maxSnippet}` : ''}${
      maxImagePreview ? `,max-image-preview:${maxImagePreview}` : ''
    }${noarchive ? ',noarchive' : ''}${unavailableAfter ? `,unavailable_after:${unavailableAfter}` : ''}${
      noimageindex ? ',noimageindex' : ''
    }${maxVideoPreview ? `,max-video-preview:${maxVideoPreview}` : ''}${notranslate ? ',notranslate' : ''}`;
  }

  $: if (!robots && additionalRobotsProps) {
    console.warn('additionalRobotsProps cannot be used when robots is set to false');
  }
</script>

<svelte:head>
  <title>{updatedTitle}</title>

  {#if robots !== false}
    <meta name="robots" content="{robots}{robotsParams}" />
  {/if}

  {#if description}
    <meta name="description" content={description} />
  {/if}

  {#if canonical}
    <link rel="canonical" href={canonical} />
  {/if}

  {#if mobileAlternate}
    <link rel="alternate" media={mobileAlternate.media} href={mobileAlternate.href} />
  {/if}

  {#if languageAlternates && languageAlternates.length > 0}
    {#each languageAlternates as languageAlternate}
      <link rel="alternate" hrefLang={languageAlternate.hrefLang} href={languageAlternate.href} />
    {/each}
  {/if}

  {#if twitter}
    {#if twitter.cardType}
      <meta name="twitter:card" content={twitter.cardType} />
    {/if}
    {#if twitter.site}
      <meta name="twitter:site" content={twitter.site} />
    {/if}
    {#if twitter.handle}
      <meta name="twitter:creator" content={twitter.handle} />
    {/if}
    {#if twitter.title}
      <meta name="twitter:title" content={twitter.title} />
    {/if}
    {#if twitter.description}
      <meta name="twitter:description" content={twitter.description} />
    {/if}
    {#if twitter.image}
      <meta name="twitter:image" content={twitter.image} />
    {/if}
    {#if twitter.imageAlt}
      <meta name="twitter:image:alt" content={twitter.imageAlt} />
    {/if}
  {/if}

  {#if facebook}
    <meta property="fb:app_id" content={facebook.appId} />
  {/if}

  {#if openGraph}
    {#if openGraph.url || canonical}
      <meta property="og:url" content={openGraph.url || canonical} />
    {/if}

    {#if openGraph.type}
      <meta property="og:type" content={openGraph.type.toLowerCase()} />
      {#if openGraph.type.toLowerCase() === 'profile' && openGraph.profile}
        {#if openGraph.profile.firstName}
          <meta property="profile:first_name" content={openGraph.profile.firstName} />
        {/if}

        {#if openGraph.profile.lastName}
          <meta property="profile:last_name" content={openGraph.profile.lastName} />
        {/if}

        {#if openGraph.profile.username}
          <meta property="profile:username" content={openGraph.profile.username} />
        {/if}

        {#if openGraph.profile.gender}
          <meta property="profile:gender" content={openGraph.profile.gender} />
        {/if}
      {:else if openGraph.type.toLowerCase() === 'book' && openGraph.book}
        {#if openGraph.book.authors && openGraph.book.authors.length}
          {#each openGraph.book.authors as author}
            <meta property="book:author" content={author} />
          {/each}
        {/if}

        {#if openGraph.book.isbn}
          <meta property="book:isbn" content={openGraph.book.isbn} />
        {/if}

        {#if openGraph.book.releaseDate}
          <meta property="book:release_date" content={openGraph.book.releaseDate} />
        {/if}

        {#if openGraph.book.tags && openGraph.book.tags.length}
          {#each openGraph.book.tags as tag}
            <meta property="book:tag" content={tag} />
          {/each}
        {/if}
      {:else if openGraph.type.toLowerCase() === 'article' && openGraph.article}
        {#if openGraph.article.publishedTime}
          <meta property="article:published_time" content={openGraph.article.publishedTime} />
        {/if}

        {#if openGraph.article.modifiedTime}
          <meta property="article:modified_time" content={openGraph.article.modifiedTime} />
        {/if}

        {#if openGraph.article.expirationTime}
          <meta property="article:expiration_time" content={openGraph.article.expirationTime} />
        {/if}

        {#if openGraph.article.authors && openGraph.article.authors.length}
          {#each openGraph.article.authors as author}
            <meta property="article:author" content={author} />
          {/each}
        {/if}

        {#if openGraph.article.section}
          <meta property="article:section" content={openGraph.article.section} />
        {/if}

        {#if openGraph.article.tags && openGraph.article.tags.length}
          {#each openGraph.article.tags as tag}
            <meta property="article:tag" content={tag} />
          {/each}
        {/if}
      {:else if openGraph.type.toLowerCase() === 'video.movie' || openGraph.type.toLowerCase() === 'video.episode' || openGraph.type.toLowerCase() === 'video.tv_show' || (openGraph.type.toLowerCase() === 'video.other' && openGraph.video)}
        {#if openGraph.video?.actors && openGraph.video.actors.length}
          {#each openGraph.video.actors as actor}
            {#if actor.profile}
              <meta property="video:actor" content={actor.profile} />
            {/if}
            {#if actor.role}
              <meta property="video:actor:role" content={actor.role} />
            {/if}
          {/each}
        {/if}

        {#if openGraph.video?.directors && openGraph.video.directors.length}
          {#each openGraph.video.directors as director}
            <meta property="video:director" content={director} />
          {/each}
        {/if}

        {#if openGraph.video?.writers && openGraph.video.writers.length}
          {#each openGraph.video.writers as writer}
            <meta property="video:writer" content={writer} />
          {/each}
        {/if}

        {#if openGraph.video?.duration}
          <meta property="video:duration" content={openGraph.video.duration.toString()} />
        {/if}

        {#if openGraph.video?.releaseDate}
          <meta property="video:release_date" content={openGraph.video.releaseDate} />
        {/if}

        {#if openGraph.video?.tags && openGraph.video.tags.length}
          {#each openGraph.video.tags as tag}
            <meta property="video:tag" content={tag} />
          {/each}
        {/if}

        {#if openGraph.video?.series}
          <meta property="video:series" content={openGraph.video.series} />
        {/if}
      {/if}
    {/if}

    {#if openGraph.title || updatedTitle}
      <meta property="og:title" content={openGraph.title || updatedTitle} />
    {/if}

    {#if openGraph.description || description}
      <meta property="og:description" content={openGraph.description || description} />
    {/if}

    {#if openGraph.images && openGraph.images.length}
      {#each openGraph.images as image}
        <meta property="og:image" content={image.url} />
        {#if image.alt}
          <meta property="og:image:alt" content={image.alt} />
        {/if}
        {#if image.width}
          <meta property="og:image:width" content={image.width.toString()} />
        {/if}
        {#if image.height}
          <meta property="og:image:height" content={image.height.toString()} />
        {/if}
        {#if image.secureUrl}
          <meta property="og:image:secure_url" content={image.secureUrl.toString()} />
        {/if}
        {#if image.type}
          <meta property="og:image:type" content={image.type.toString()} />
        {/if}
      {/each}
    {/if}

    {#if openGraph.videos && openGraph.videos.length}
      {#each openGraph.videos as video}
        <meta property="og:video" content={video.url} />
        {#if video.width}
          <meta property="og:video:width" content={video.width.toString()} />
        {/if}
        {#if video.height}
          <meta property="og:video:height" content={video.height.toString()} />
        {/if}
        {#if video.secureUrl}
          <meta property="og:video:secure_url" content={video.secureUrl.toString()} />
        {/if}
        {#if video.type}
          <meta property="og:video:type" content={video.type.toString()} />
        {/if}
      {/each}
    {/if}

    {#if openGraph.audio && openGraph.audio.length}
      {#each openGraph.audio as audio}
        <meta property="og:audio" content={audio.url} />
        {#if audio.secureUrl}
          <meta property="og:audio:secure_url" content={audio.secureUrl.toString()} />
        {/if}
        {#if audio.type}
          <meta property="og:audio:type" content={audio.type.toString()} />
        {/if}
      {/each}
    {/if}

    {#if openGraph.locale}
      <meta property="og:locale" content={openGraph.locale} />
    {/if}

    {#if openGraph.siteName}
      <meta property="og:site_name" content={openGraph.siteName} />
    {/if}
  {/if}

  {#if additionalMetaTags && Array.isArray(additionalMetaTags)}
    {#each additionalMetaTags as tag}
      <meta {...tag} />
    {/each}
  {/if}

  {#if additionalLinkTags?.length}
    {#each additionalLinkTags as tag}
      <link {...tag} />
    {/each}
  {/if}

  {#if keywords?.length}
    <meta name="keywords" content={keywords.join(', ')} />
  {/if}
</svelte:head>
