import random
import re
import string
import lyricsgenius as lg
from pyrogram import filters
from pyrogram.types import (InlineKeyboardButton,
                            InlineKeyboardMarkup, Message)

from config import BANNED_USERS, lyrical
import strings
from AnieXEricaMusic import app
from AnieXEricaMusic.utils.decorators.language import language

api_key = "Vd9FvPMOKWfsKJNG9RbZnItaTNIRFzVyyXFdrGHONVsGqHcHBoj3AI3sIlNuqzuf0ZNG8uLcF9wAd5DXBBnUzA"
y = lg.Genius(
    api_key,
    skip_non_songs=True,
    excluded_terms=["(Remix)", "(Live)"],
    remove_section_headers=True,
)
y.verbose = False


@app.on_message(filters.command(["lyrics"],  prefixes=["+", ".", "/", "-", "", "$","#","&"]))
@language
async def lrsearch(client, message: Message, _):
    if len(message.command) < 2:
        return await message.reply_text(_["rs_1"])
    title = message.text.split(None, 1)[1]
    m = await message.reply_text(_["rs_2"])
    S = y.search_song(title, get_full_info=False)
    if S is None:
        return await m.edit(_["rs_3"].format(title))
    ran_hash = "".join(
        random.choices(string.ascii_uppercase + string.digits, k=10)
    )
    lyric = S.lyrics
    if "Embed" in lyric:
        lyric = re.sub(r"\d*Embed", "", lyric)
    lyrical[ran_hash] = lyric
    upl = InlineKeyboardMarkup(
        [
            [
                InlineKeyboardButton(
                    text=_["A_M_B"],
                    url=f"https://t.me/{app.username}?start=lyrics_{ran_hash}",
                ),
            ]
        ]
    )
    await m.edit(_["rs_4"], reply_markup=upl)
