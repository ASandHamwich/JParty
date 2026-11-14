# How to set up and play JParty

JParty turns a laptop and a TV into a Jeopardy!-style game show. One person hosts from the laptop, everyone else buzzes in from their phone. No technical knowledge is needed.

## What you need

- A computer (Mac, Windows or Linux) **connected to a second screen** such as a TV or projector. JParty will not start with only one screen.
- Wi-Fi with internet access, shared by the computer and every player's phone.
- One phone per player (1 to 8 players; 3 is the classic number).
- One person to be the host. The host reads the clues and judges the answers, and does not play.

## Step 1: Get the app

Download the file for your computer from the [Releases page](https://github.com/stuartthomas25/JParty/releases), unzip it, and open it.

- **Mac:** the app is not signed by Apple, so the first launch will probably be blocked. Right-click the app and choose **Open**, or go to System Settings → Privacy & Security and click **Open Anyway**.
- If your computer asks whether JParty may accept incoming network connections, say **Allow**. The phones cannot connect otherwise.

Prefer to run it from the source code? See [Running from source](#running-from-source) at the bottom.

## Step 2: Set up the screens

1. Plug the computer into the TV.
2. Turn **off** screen mirroring. The two screens must show different things.
3. Keep the laptop as the main (primary) display. The laptop shows the host view with the answers; the TV shows the board for the players.
4. Use a cable if you can. Wireless casting (AirPlay and similar) works but makes the buzzers feel laggy.

## Step 3: Start a game

1. Open JParty. Theme music plays, the laptop shows the welcome screen and the TV shows a QR code.
2. **Players:** connect to the same Wi-Fi as the computer, scan the QR code (or type the address shown under it), sign or type a name, and tap **Play!**. A podium appears on screen for each player.
3. **Host:** choose a game on the laptop:
   - Click **Random** for a random real episode, or
   - Type a **Game ID** from [J-Archive](https://j-archive.com). It is the number at the end of an episode's web address, e.g. `showgame.php?game_id=`**`7000`**, or
   - Paste a Google Sheet ID to play [your own questions](#playing-your-own-questions).
4. When the episode's date appears under the box and at least one player has joined, click **Start!**

## Step 4: Run the game (host)

The host uses only the mouse, the **space bar**, and the **left / right arrow keys**. The edges of the host screen flash a picture of the key that is expected next.

### A normal clue

| Step | What the host does | What happens |
| --- | --- | --- |
| 1 | Click a dollar amount on the board | The clue appears on both screens. The answer appears on the laptop only. |
| 2 | Read the clue aloud, then press **Space** | The screen edges light up white: buzzers are open. |
| 3 | The first player to buzz answers out loud | Their podium lights up and counts down 5 seconds. |
| 4 | Press **←** if they were right, **→** if they were wrong | Right: they win the money and the board returns. Wrong: they lose the money and buzzers reopen for the others. |
| 5 | If nobody buzzes within about 4 seconds, a time-out sound plays. Press **Space** | Back to the board. |

Remember: **← left = correct (green), → right = wrong (red).**

### Daily Double

1. The TV shows "DAILY DOUBLE!". On the laptop, **click the podium** of the player who picked the clue.
2. Ask for their wager and type it in, or click **True Daily Double!** to bet everything.
3. The clue appears. Read it, hear their answer, then press **←** or **→**.

### Moving between rounds

When every clue is gone, press **Space** to go to the next round. To skip ahead (or go back) at any time, click the **gear icon** in the top-right corner of the host screen and choose **Next round** or **Previous round**.

### Final round

1. Each phone asks its player for a wager. Podiums stay lit until that player has wagered.
2. When everyone has wagered, press **Space** to show the clue and read it aloud.
3. Press **Space** again. The 30-second "think" music plays and the phones ask for a written answer. Whatever is typed when the music ends is submitted automatically.
4. Press **Space** to step through the players, lowest score first. For each one: **Space** reveals their answer, then **←** / **→** judges it and shows the wager.
5. After the last player the winner is shown. Press **Space** to return to the welcome screen.

## Fixing things mid-game

- **Wrong score?** Click the player's podium on the laptop → **Change score**.
- **Remove a player?** Click their podium → **Remove player**.
- **Latecomer wants to join?** Gear icon → **Allow new players**. The QR code comes back on the TV. Click **Done** when they are in.
- **A phone lost its connection or went to sleep?** Reload the page on that phone. It rejoins as the same player with the same score (for up to 24 hours).
- **End the game early?** Gear icon → **Quit game**.

## Playing your own questions

1. Open the [Google Sheets template](https://docs.google.com/spreadsheets/d/1JqfJ_OgTstaXTyH5nV3_eN6YXqvZ1RviPmN7OwLhG0U/edit?usp=sharing) and make your own copy (File → Make a copy).
2. Fill in your categories, clues and answers. Keep the layout exactly as it is: 6 categories by 5 clues, for two rounds, plus one final clue.
3. Click Share and set access to **Anyone with the link can view**.
4. Copy the Sheet ID: the long string of letters and numbers between `/d/` and `/edit` in the sheet's web address.
5. Paste it into the Game ID box in JParty.

Pictures are not supported.

## Troubleshooting

| Problem | What to do |
| --- | --- |
| "Two monitors needed!" | Connect a second screen and turn off mirroring, then reopen JParty. |
| "Cannot connect!" | The computer has no internet. JParty needs it to download the questions. |
| The QR code does not open anything | Make sure the phone is on the same Wi-Fi as the computer. Guest and public networks often block devices from seeing each other; try another network, or connect everything to one phone's hotspot. Also check that the computer's firewall allows JParty. |
| The phone page loads but looks broken | The phone needs internet access too, not just the local Wi-Fi. |
| Phone says "Game has started!" | The host must click the gear icon → **Allow new players**. |
| **Start!** is greyed out | You need both a loaded game (a date shows under the Game ID box) and at least one player. |
| "Game is incomplete" | That episode has missing clues on J-Archive. Pick another ID or click Random. |
| "Cannot get game" | The ID is wrong, or the Google Sheet is not shared publicly. |
| Buzzer goes dark | On the phone, turn off Auto-Lock and Low Power Mode. |
| Buzzers feel slow | Connect the TV with a cable instead of wireless casting. |

## Running from source

Only needed if you want to change the code or there is no download for your computer. You need Python installed; use **Python 3.11**, which is the version the project's official builds use. Newer versions may fail to install the audio library.

```
git clone https://github.com/stuartthomas25/JParty.git
cd JParty
python3.11 -m venv .venv
source .venv/bin/activate        # on Windows: .venv\Scripts\activate
pip install -r requirements.txt
cd jparty
python ../run.py
```

The last two lines matter: JParty must be started **from inside the `jparty` folder**, or it will not find its sounds and images.

On Linux, install the audio headers first: `sudo apt-get install libasound2-dev`.

To build a standalone app yourself, run `pyinstaller -y JParty.spec` from the top folder. The result appears in `dist/`.
