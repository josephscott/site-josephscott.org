---
title: Percentiles
---

<style>
.container {
	display: grid;
	grid-template-columns: 0.7fr 1.3fr;
	grid-template-rows: 1fr 1fr;
	gap: 0px 1em;
	grid-template-areas:
		". ."
		". .";
}
.numbers {
	height: 405px;
}
</style>

<p>
Provide a space or new line separated list of numbers.
</p>

<div class="container">
	<textarea class="numbers" cols="35" rows="10" autofocus></textarea>
	<div class="results"></div>
</div>

<script>
document.addEventListener( 'DOMContentLoaded', function() {
	let $qs = document.querySelector.bind( document );

	let numbers_el = $qs( '.numbers' );
	let results_el = $qs( '.results' );

	let do_percentiles = ( event ) => {
		let out = '';
		let levels = [ 25, 50, 75, 90, 95, 99 ];
		let numbers = event.target.value.trim().split( /\s+/ );

		numbers.forEach( ( num, i ) => {
			let parsed_num = Number( num );
			if ( !isNaN( parsed_num ) ) {
				numbers[i] = parsed_num;
			} else {
				delete numbers[i];
			}
		} );

		numbers.sort( ( a, b ) => { return a - b } );

		out += " --- Interpolated ---\n"
		levels.forEach( ( p ) => {
			p_decimal = p / 100;

			let index = p_decimal * ( numbers.length - 1 ),
				lower = Math.floor( index ),
				remainder = index - lower;

			let interp = numbers[lower];
			if ( numbers[lower + 1] !== undefined ) {
				interp = numbers[lower] + (
					remainder * ( numbers[lower + 1] - numbers[lower] )
				);
			}

			interp = new Intl.NumberFormat( 'en-US', {} ).format( interp );
			out += `p${p} = ${interp}\n`;
		} );

		out += " --- Ranked ---\n";
		levels.forEach( ( p ) => {
			p_decimal = p / 100;

			let index = p_decimal * numbers.length;
			index = Math.floor( index );
			let ranked = numbers[index];

			ranked = new Intl.NumberFormat( 'en-US', {} ).format( ranked );
			out += `p${p} = ${ranked}\n`;
		} );

		results_el.innerText = out;
	}

	numbers_el.addEventListener( 'input', do_percentiles );
} );
</script>
