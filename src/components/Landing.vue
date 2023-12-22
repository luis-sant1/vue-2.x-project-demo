<!-- Imports -->
<script>
import { VueEditor } from "vue2-editor";
import $ from "jquery";

export default {
    name: 'LandingPage',
    components: {
        VueEditor
    },
    data() {
        return {
            showModal: false,
            content: `
            <h1> this header </h1>
            <p>Amet irure excepteur cillum ipsum sit elit dolor et nisi eu consectetur aliqua laborum laboris.</p>
            <h1> This Footer</h1>
            `
        }
    },
    methods: {
            Export2Doc(element, filename = '') {
               
            //  _html_ will be replace with custom html
            var meta= "Mime-Version: 1.0\nContent-Base: " + location.href + "\nContent-Type: Multipart/related; boundary=\"NEXT.ITEM-BOUNDARY\";type=\"text/html\"\n\n--NEXT.ITEM-BOUNDARY\nContent-Type: text/html; charset=\"utf-8\"\nContent-Location: " + location.href + "\n\n<!DOCTYPE html>\n<html>\n_html_</html>";
            //  _styles_ will be replaced with custome css
            var head= "<head>\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\n<style>\n_styles_\n</style>\n</head>\n";
  
            // var html = document.getElementById(element).innerHTML ;
            
            var html = this.content ;
            
            var blob = new Blob(['\ufeff', html], {
                type: 'application/msword'
            });
            
            var  css = (
                   '<style>' +
                   'img {width:300px;}table {border-collapse: collapse; border-spacing: 0;}td{padding: 6px;}' +
                   '</style>'
                  );
//  Image Area %%%%
            var options = { maxWidth: 624};
            var images = Array();
            var img = $("#" + element).find("img");
            for (var i = 0; i < img.length; i++) {
                // Calculate dimensions of output image
                var w = Math.min(img[i].width, options.maxWidth);
                var h = img[i].height * (w / img[i].width);
                // Create canvas for converting image to data URL
                var canvas = document.createElement("CANVAS");
                canvas.width = w;
                canvas.height = h;
                // Draw image to canvas
                var context = canvas.getContext('2d');
                context.drawImage(img[i], 0, 0, w, h);
                // Get data URL encoding of image
                var uri = canvas.toDataURL("image/png");
                $(img[i]).attr("src", img[i].src);
                img[i].width = w;
                img[i].height = h;
                // Save encoded image to array
                images[i] = {
                    type: uri.substring(uri.indexOf(":") + 1, uri.indexOf(";")),
                    encoding: uri.substring(uri.indexOf(";") + 1, uri.indexOf(",")),
                    location: $(img[i]).attr("src"),
                    data: uri.substring(uri.indexOf(",") + 1)
                };
            }

            // Prepare bottom of mhtml file with image data
            var imgMetaData = "\n";
            for (let i = 0; i < images.length; i++) {
                imgMetaData += "--NEXT.ITEM-BOUNDARY\n";
                imgMetaData += "Content-Location: " + images[i].location + "\n";
                imgMetaData += "Content-Type: " + images[i].type + "\n";
                imgMetaData += "Content-Transfer-Encoding: " + images[i].encoding + "\n\n";
                imgMetaData += images[i].data + "\n\n";
                
            }
            imgMetaData += "--NEXT.ITEM-BOUNDARY--";
// end Image Area %%

             var output = meta.replace("_html_", head.replace("_styles_", css) +  html) + imgMetaData;

            var url = 'data:application/vnd.ms-word;charset=utf-8,' + encodeURIComponent(output);


            filename = filename ? filename + '.doc' : 'document.doc';


            var downloadLink = document.createElement("a");

            document.body.appendChild(downloadLink);

            if (navigator.msSaveOrOpenBlob) {
                navigator.msSaveOrOpenBlob(blob, filename);
            } else {

                downloadLink.href = url;
                downloadLink.download = filename;
                downloadLink.click();
            }

            document.body.removeChild(downloadLink);
        },
        seeContent(){
            console.log(this.content)
        }
    }

}
</script>

<template>
    <main id="container">
        <h1>
            Demo AMII
        </h1>

        <div id="app">
            <VueEditor v-model="content" />
        </div>
        <button id="container-sendBtn" @click="showModal = true">Send</button>

        <div v-if="showModal" id="container-modal">
            <div id="container-modal-options">
                <span>Cómo quiere proceder?</span>
                <button>Imprimir documento</button>
                <button @click="Export2Doc('app', 'new-doc')">Editar documento en formato DOC</button>
                <button @click="showModal = false">Close</button>
            </div>
        </div>
    </main>
</template>



<style scoped>
#container-sendBtn {
    width: 25%;
}

#container {
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

#container-modal {
    /* display: none; */
    height: 100%;
    width: 100%;
    position: absolute;
    background-color: rgba(0, 0, 0, 0.438);
    overflow: hidden;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

#container-modal-options {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: white;
    width: 50rem;
    height: 18rem;
}
</style>