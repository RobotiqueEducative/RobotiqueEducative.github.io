# RobotiqueEducative.github.io

# A first-level heading
## A second-level heading
### A third-level heading

```python 
from hub import port
import force_sensor
import motor_pair
import runloop

motor_pair.pair(motor_pair.PAIR_1, port.C, port.D)

def pressed_detected():
    return force_sensor.pressed(port.A) == True

async def main():
    while True:
        if pressed_detected():
            motor_pair.move_for_time(motor_pair.PAIR_1, 3000, 0, velocity=200)

runloop.run(main())
```


@@ -1,31 +1,25 @@
import React from 'dom-chef';
import select from 'select-dom';
import delegate from 'delegate-it';
import copyToClipboard from 'copy-text-to-clipboard';
import features from '../libs/features';

function handleClick({currentTarget: button}: React.MouseEvent<HTMLButtonElement>): void {
	const file = button.closest('.Box');

	const content = select.all('.blob-code-inner', file!)
		.map(blob => blob.innerText) // Must be `.innerText`
		.map(line => line === '\n' ? '' : line)
		.map(({innerText: line}) => line === '\n' ? '' : line) // Must be `.innerText`
		.join('\n');

	copyToClipboard(content);
}

function init(): void {
	// This selector skips binaries + markdowns with code
	for (const code of select.all('.blob-wrapper > .highlight:not(.rgh-copy-file)')) {
		code.classList.add('rgh-copy-file');
		code
			.closest('.Box')! // Closest common container
			.querySelector('[data-hotkey="b"]')! // Easily-found `Blame` button
function renderButton(): void {
	for (const blameButton of select.all('[data-hotkey="b"]')) {
		blameButton
			.parentElement! // `BtnGroup`
			.prepend(
				<button
					onClick={handleClick}
					className="btn btn-sm copy-btn tooltipped tooltipped-n BtnGroup-item"
					className="btn btn-sm tooltipped tooltipped-n BtnGroup-item rgh-copy-file"
					aria-label="Copy file to clipboard"
					type="button">
					Copy
@@ -34,6 +28,20 @@ function init(): void {
	}
}

function init(): void {
	if (select.exists('.blob.instapaper_body')) {
		delegate('.rgh-md-source', 'rgh:view-markdown-source', renderButton);
		delegate('.rgh-md-source', 'rgh:view-markdown-rendered', () => {
			const button = select('.rgh-copy-file');
			if (button) {
				button.remove();
			}
		});
	} else {
		renderButton();
	}
}

features.add({
	id: 'copy-file',
	include: [
