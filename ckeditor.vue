<template>
	<ckeditor v-model="editorData" class="editor" @ready="onReady" @input="onInput" :editor="editor" :config="editorConfig"></ckeditor>
</template>

<script>
import ClassicEditor from "@ckeditor/ckeditor5-editor-classic/src/classiceditor";
import EssentialsPlugin from "@ckeditor/ckeditor5-essentials/src/essentials";
import AutoformatPlugin from "@ckeditor/ckeditor5-autoformat/src/autoformat";
import BoldPlugin from "@ckeditor/ckeditor5-basic-styles/src/bold";
import Code from "@ckeditor/ckeditor5-basic-styles/src/code";
import UnderlinePlugin from "@ckeditor/ckeditor5-basic-styles/src/underline";
import StrikethroughPlugin from "@ckeditor/ckeditor5-basic-styles/src/strikethrough";
import SubscriptPlugin from "@ckeditor/ckeditor5-basic-styles/src/subscript";
import SuperscriptPlugin from "@ckeditor/ckeditor5-basic-styles/src/superscript";
// import OutdentPlugin from "@ckeditor/ckeditor5-code-block/src/outdentcodeblockcommand";
import IndentPlugin from "@ckeditor/ckeditor5-indent/src/indent";
import IndentBlock from "@ckeditor/ckeditor5-indent/src/indentblock";
import FontPlugin from "@ckeditor/ckeditor5-font/src/font";
import ItalicPlugin from "@ckeditor/ckeditor5-basic-styles/src/italic";
import BlockQuotePlugin from "@ckeditor/ckeditor5-block-quote/src/blockquote";
import HeadingPlugin from "@ckeditor/ckeditor5-heading/src/heading";
import ImagePlugin from "@ckeditor/ckeditor5-image/src/image";
import ImageCaptionPlugin from "@ckeditor/ckeditor5-image/src/imagecaption";
import ImageStylePlugin from "@ckeditor/ckeditor5-image/src/imagestyle";
import ImageToolbarPlugin from "@ckeditor/ckeditor5-image/src/imagetoolbar";
import ImageUploadPlugin from "@ckeditor/ckeditor5-image/src/imageupload";
import LinkPlugin from "@ckeditor/ckeditor5-link/src/link";
import ListStyle from "@ckeditor/ckeditor5-list/src/liststyle";
import TodoList from "@ckeditor/ckeditor5-list/src/todolist";
import ParagraphPlugin from "@ckeditor/ckeditor5-paragraph/src/paragraph";
import Alignment from "@ckeditor/ckeditor5-alignment/src/alignment";
import Clipboard from "@ckeditor/ckeditor5-clipboard/src/clipboard";
import ImageInsert from "@ckeditor/ckeditor5-image/src/imageinsert";
import AutoImage from "@ckeditor/ckeditor5-image/src/autoimage";
import ImageResize from "@ckeditor/ckeditor5-image/src/imageresize";
import SimpleUploadAdapter from "@ckeditor/ckeditor5-upload/src/adapters/simpleuploadadapter";
import ImageRemoveEventCallbackPlugin from "ckeditor5-image-remove-event-callback-plugin";
import CodeBlock from "@ckeditor/ckeditor5-code-block/src/codeblock";
import InsertTableView from "@ckeditor/ckeditor5-table/src/table";
import Autoformat from "@ckeditor/ckeditor5-autoformat/src/autoformat";

export default {
	name: "app",
	data() {
		return {
			editor: ClassicEditor,
			editorData: "",
			editorConfig: {
				plugins: [
					EssentialsPlugin,
					AutoformatPlugin,
					BoldPlugin,
					UnderlinePlugin,
					ItalicPlugin,
					BlockQuotePlugin,
					HeadingPlugin,
					ImagePlugin,
					ImageCaptionPlugin,
					ImageStylePlugin,
					ImageToolbarPlugin,
					ImageUploadPlugin,
					LinkPlugin,
					ListStyle,
					ParagraphPlugin,
					Alignment,
					Clipboard,
					ImageInsert,
					AutoImage,
					ImageResize,
					SimpleUploadAdapter,
					ImageRemoveEventCallbackPlugin,
					FontPlugin,
					StrikethroughPlugin,
					SubscriptPlugin,
					SuperscriptPlugin,
					// OutdentPlugin,
					IndentPlugin,
					IndentBlock,
					TodoList,
					Code,
					CodeBlock,
					InsertTableView,
					Autoformat,
				],

				toolbar: {
					items: [
						"heading",
						"|",
						"fontfamily",
						"fontsize",
						"|",
						"alignment",
						"|",
						"fontColor",
						"fontBackgroundColor",
						"|",
						"bold",
						"italic",
						"strikethrough",
						"underline",
						"subscript",
						"superscript",
						"|",
						"link",
						"|",
						"outdent",
						"indent",
						"|",
						"code",
						"codeBlock",
						"|",
						"insertTable",
						"|",
						"imageInsert",
						"blockQuote",
						"|",
						"undo",
						"redo",
						"|",
						"numberedList",
						"bulletedList",
					],
					shouldNotGroupWhenFull: true,
					viewportTopOffset: 30,
				},
				image: {
					toolbar: ["imageStyle:full", "imageStyle:side", "|", "imageTextAlternative"],
				},
				cloudServices: {
					uploadUrl: "",
				},
				simpleUpload: {
					// The URL that the images are uploaded to.
					uploadUrl: "",

					// Enable the XMLHttpRequest.withCredentials property.
					withCredentials: false,

					// Headers sent along with the XMLHttpRequest to the upload server.
					headers: {
						"X-CSRF-TOKEN": "CSRF-Token",
						Authorization: "",
					},
				},
			},
		};
	},
	async created() {
		this.editorConfig.simpleUpload.headers.Authorization = `Bearer ${localStorage.getItem("barter-token")}`;
		this.editorConfig.simpleUpload.uploadUrl = `http://127.0.0.1:3333/api/barter/s3/editorimage/upload/`;
		this.editorConfig.imageRemoveEvent = { callback: this.imageRemove };
	},
	mounted() {},
	props: {
		onReady: {
			type: Function,
			required: false,
		},
	},
	methods: {
		onInput() {
			this.$emit("editorInput", this.editorData);
		},
		imageRemove(images) {
		/** HANDLE THE IMAGE DELETION IN YOUR PARENT COMPONENT */
			this.$emit("imageRemove", images);
		},
	},
};
</script>

<style scoped>
:not(ol, ul, li, ) {
	margin: 0;
	padding: 0;
}
</style>
