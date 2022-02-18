---
title: Percentiles
---

<p>
Provide a space or new line separated list of numbers.
</p>

<textarea class="numbers" cols="35" rows="10"></textarea>

<p class="results"></p>

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
