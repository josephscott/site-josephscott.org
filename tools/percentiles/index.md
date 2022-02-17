---
title: Percentiles
---

<p>
Provide a space or new line separated list of numbers.
</p>

<textarea class="numbers" cols="35" rows="10"></textarea>

<p class="results"></p>

<script>
document.addEventListener("DOMContentLoaded", function() {
	let $qs = document.querySelector.bind( document );

	let numbers_el = $qs( '.numbers' );
	let results_el = $qs( '.results' );

	let do_percentiles = ( event ) => {
		let out = '';
		let numbers = event.target.value.trim().split( /\s+/ );

		numbers.forEach( ( num, i ) => {
			let parsed_num = Number( num );
			if ( !isNaN( parsed_num ) ) {
				numbers[i] = parsed_num;
			} else {
				delete numbers[i];
			}
		} );

		[ 25, 50, 75, 90, 95 ].forEach( ( p ) => {
			numbers.sort( ( a, b ) => { return a - b } );
			let index = ( p / 100 ) * numbers.length;
			index = Math.floor( index );

			out += `p${p} = ${numbers[index]}\n`;
		} );

		results_el.innerText = out;
	}

	numbers_el.addEventListener( 'input', do_percentiles );
});
</script>
