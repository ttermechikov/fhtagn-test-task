<template>
  <b-button variant="outline-primary" @click="handleClick">Экспорт</b-button>
</template>

<script>
export default {
  name: 'DownloadXml',
  methods: {
    handleClick() {
      this.createXmlFile(this.posts);
    },
    createXmlFile(posts = []) {
      let xmltext = '<?xml version="1.0" encoding="UTF-8" ?><Posts>';
      posts.forEach(({ id, userId, title, body }) => {
        xmltext += `<Post>
                <Id>${id}</Id>
                <UserId>${userId}</UserId>
                <Title>${title}</Title>
                <Body>${body}</Body>
            </Post>`;
      });
      xmltext += '</Posts>';

      const filename = `${Date.now()}.xml`;
      const pom = document.createElement('a');
      const bb = new Blob([xmltext], { type: 'text/plain' });

      pom.setAttribute('href', window.URL.createObjectURL(bb));
      pom.setAttribute('download', filename);

      pom.dataset.downloadurl = ['text/plain', pom.download, pom.href].join(
        ':' // eslint-disable-line
      );
      pom.draggable = true;
      pom.classList.add('dragout');

      pom.click();
    },
  },
  props: {
    posts: Array,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
