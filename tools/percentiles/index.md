---
title: Percentiles
---

<style>
.numbers {
	width: 90%;
}
</style>

<p>
Provide a space or new line separated list of numbers.
</p>

<textarea class="numbers" rows="10"></textarea>

<p class="results"></p>

<script>
document.addEventListener("DOMContentLoaded", function() {
	let $qs = document.querySelector.bind( document );

	let numbers_el = $qs( '.numbers' );
	let results_el = $qs( '.results' );

	let do_percentiles = ( event ) => {
		let p = 50;
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
		let index = ( p / 100 ) * numbers.length;
		index = Math.floor( index );

		results_el.innerText = `p50 = ${numbers[index]}`;
	}

	numbers_el.addEventListener( 'input', do_percentiles );
});
</script>
