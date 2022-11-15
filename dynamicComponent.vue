<script lang="js" frontend>
/**
* Extended version of the built in `<script lang="js" frontend>` dynamic component that supports events and props and sanity checks
*
* @param {string} component The component name if passed a string (the component name is always camelCased so you can pass in dotted, kebab case of any other varient)
* @param {Object} [attrs] Unbound attributes as an object lookup
* @param {Object} [props] Prop bindings as an object lookup
* @param {Object} [events] Event bindings as an object lookup
*
* NOTE: This component exposes a single ref `component` which is the dynamically loaded component
*/
app.component('dynamicComponent', {
	props: {
		component: {
			type: String,
			required: true,
			validator: val => {
				var isValid = app.component(_.camelCase(val));
				if (!isValid) console.error('Dynamic component', val, 'is not available via app.component()');
				return isValid;
			},
		},
		props: {type: Object},
		events: {type: Object},
	},
	render: function(h) {
		return h(_.camelCase(this.$props.component), {
			ref: 'component',
			key: _.camelCase(this.$props.component),
			attrs: this.attrs,
			props: this.props,
			on: this.events,
		});
	},
});
</script>
