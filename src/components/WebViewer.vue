<template>
  <div ref="viewer"></div>
</template>

<script>
/* eslint-disable */
import WebViewer from "@pdftron/pdfjs-express-viewer";

export default {
  name: "WebViewer",
  props: {
    path: String,
  },
  mounted: function () {
    WebViewer(
      {
        path: this.path,
        initialDoc: `${process.env.BASE_URL}files/demo.pdf`,
        licenseKey: 'VMeLR5MsW5lX3X9YfqQF',
      },
      this.$refs.viewer
    ).then((instance) => {
      // now you can access APIs through the WebViewer instance
      const { Core, UI } = instance;

      // adding an event listener for when a document is loaded
      Core.documentViewer.addEventListener("documentLoaded", () => {
        console.log("document loaded");
      });

      // adding an event listener for when the page number has changed
      Core.documentViewer.addEventListener(
        "pageNumberUpdated",
        (pageNumber) => {
          console.log(`Page number is: ${pageNumber}`);
        }
      );

      // adds a button to the header that on click sets the page to the next page
      UI.setHeaderItems((header) => {
        header.push({
          type: "actionButton",
          img: "https://icons.getbootstrap.com/assets/icons/caret-right-fill.svg",
          onClick: () => {
            const currentPage = Core.documentViewer.getCurrentPage();
            const totalPages = Core.documentViewer.getPageCount();
            const atLastPage = currentPage === totalPages;

            if (atLastPage) {
              Core.documentViewer.setCurrentPage(1);
            } else {
              Core.documentViewer.setCurrentPage(currentPage + 1);
            }
          },
        });
      });
    });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div {
  width: 100%;
  height: 100vh;
}
</style>
