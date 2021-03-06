# Gifted Chat

Web fork of most complete chat UI for React Web (formerly known as Gifted Messenger).

## Demo is available [here](https://codesandbox.io/s/pw6zzl542x)

![](https://thumbs.gfycat.com/VibrantRecklessIriomotecat-size_restricted.gif)

## Article
The article how to create working chat in 20 minutes is available on medium - [here](https://medium.com/@janromaniak/working-realtime-chat-in-react-in-less-than-20-minutes-335f9cf73d69)

## Features

- Fully customizable components
- Composer actions (to attach photos, etc.)
- Load earlier messages
- Copy messages to clipboard
- Touchable links using [react-native-parsed-text](https://github.com/taskrabbit/react-native-parsed-text)
- Avatar as user's initials
- Localized dates
- Multiline TextInput
- InputToolbar avoiding keyboard


## Installation

- Using [npm](https://www.npmjs.com/#getting-started): `npm install react-web-gifted-chat --save`
- Using [Yarn](https://yarnpkg.com/): `yarn add react-web-gifted-chat`

## Example

```jsx
import { GiftedChat } from 'react-web-gifted-chat';

class Example extends React.Component {

  state = {
    messages: [],
  };

  componentWillMount() {
    this.setState({
      messages: [
        {
          id: 1,
          text: 'Hello developer',
          createdAt: new Date(),
          user: {
            id: 2,
            name: 'React',
            avatar: 'https://facebook.github.io/react/img/logo_og.png',
          },
        },
      ],
    });
  }

  onSend(messages = []) {
    this.setState((previousState) => ({
      messages: GiftedChat.append(previousState.messages, messages),
    }));
  }

  render() {
    return (
      <GiftedChat
        messages={this.state.messages}
        onSend={(messages) => this.onSend(messages)}
        user={{
          id: 1,
        }}
      />
    );
  }

}
```

## Advanced example

See [example/App.js](example/App.js) for a working demo!

## Message object

e.g.

```js
{
  id: 1,
  text: 'My message',
  createdAt: new Date(Date.UTC(2016, 5, 11, 17, 20, 0)),
  user: {
    id: 2,
    name: 'React',
    avatar: 'https://facebook.github.io/react/img/logo_og.png',
  },
  image: 'https://facebook.github.io/react/img/logo_og.png',
  // Any additional custom parameters are passed through
}
```

## Props

- **`messages`** _(Array)_ - Messages to display
- **`text`** _(String)_ - Input text; default is `undefined`, but if specified, it will override GiftedChat's internal state (e.g. for redux; [see notes below](#notes-for-redux))
- **`placeholder`** _(String)_ - Placeholder when `text` is empty; default is `'Type a message...'`
- **`messageIdGenerator`** _(Function)_ - Generate an id for new messages. Defaults to UUID v4, generated by [uuid](https://github.com/kelektiv/node-uuid)
- **`user`** _(Object)_ - User sending the messages: `{ _id, name, avatar }`
- **`onSend`** _(Function)_ - Callback when sending a message
- **`locale`** _(String)_ - Locale to localize the dates
- **`timeFormat`** _(String)_ - Format to use for rendering times; default is `'LT'`
- **`dateFormat`** _(String)_ - Format to use for rendering dates; default is `'ll'`
- **`isAnimated`** _(Bool)_ - Animates the view when the keyboard appears
- **`loadEarlier`** _(Bool)_ - Enables the "Load earlier messages" button
- **`onLoadEarlier`** _(Function)_ - Callback when loading earlier messages
- **`isLoadingEarlier`** _(Bool)_ - Display an `ActivityIndicator` when loading earlier messages
- **`renderLoading`** _(Function)_ - Render a loading view when initializing
- **`renderLoadEarlier`** _(Function)_ - Custom "Load earlier messages" button
- **`renderAvatar`** _(Function)_ - Custom message avatar; set to `null` to not render any avatar for the message
- **`showUserAvatar`** _(Bool)_ - Whether to render an avatar for the current user; default is `false`, only show avatars for other users
- **`showAvatarForEveryMessage`** _(Bool)_ - When false, avatars will only be displayed when a consecutive message is from the same user on the same day; default is `false`
- **`onPressAvatar`** _(Function(`user`))_ - Callback when a message avatar is tapped
- **`renderAvatarOnTop`** _(Bool)_ - Render the message avatar at the top of consecutive messages, rather than the bottom; default is `false`
- **`renderBubble`** _(Function)_ - Custom message bubble
- **`renderSystemMessage`** _(Function)_ - Custom system message
- **`onLongPress`** _(Function(`context`, `message`))_ - Callback when a message bubble is long-pressed; default is to show an ActionSheet with "Copy Text" (see [example using `showActionSheetWithOptions()`](https://github.com/FaridSafi/react-native-gifted-chat/blob/master@%7B2017-09-25%7D/src/Bubble.js#L96-L119))
- **`inverted`** _(Bool)_ - Reverses display order of `messages`; default is `true`
- **`renderMessage`** _(Function)_ - Custom message container
- **`renderMessageText`** _(Function)_ - Custom message text
- **`renderMessageImage`** _(Function)_ - Custom message image
- **`imageProps`** _(Object)_ - Extra props to be passed to the [`<Image>`](https://facebook.github.io/react-native/docs/image.html) component created by the default `renderMessageImage`
- **`videoProps`** _(Object)_ - Extra props to be passed to the [`<Video>`](https://github.com/react-native-community/react-native-video) component created by the default `renderMessageVideo`
- **`lightboxProps`** _(Object)_ - Extra props to be passed to the `MessageImage`'s [Lightbox](https://github.com/oblador/react-native-lightbox)
- **`renderCustomView`** _(Function)_ - Custom view inside the bubble
- **`renderDay`** _(Function)_ - Custom day above a message
- **`renderTime`** _(Function)_ - Custom time inside a message
- **`renderFooter`** _(Function)_ - Custom footer component on the ListView, e.g. `'User is typing...'`; see [example/App.js](example/App.js) for an example
- **`renderChatFooter`** _(Function)_ - Custom component to render below the MessageContainer (separate from the ListView)
- **`renderInputToolbar`** _(Function)_ - Custom message composer container
- **`renderComposer`** _(Function)_ - Custom text input message composer
- **`renderActions`** _(Function)_ - Custom action button on the left of the message composer
- **`renderSend`** _(Function)_ - Custom send button; you can pass children to the original `Send` component quite easily, for example to use a custom icon ([example](https://github.com/FaridSafi/react-native-gifted-chat/pull/487))
- **`renderAccessory`** _(Function)_ - Custom second line of actions below the message composer
- **`onPressActionButton`** _(Function)_ - Callback when the Action button is pressed (if set, the default `actionSheet` will not be used)
- **`bottomOffset`** _(Integer)_ - Distance of the chat from the bottom of the screen (e.g. useful if you display a tab bar)
- **`minInputToolbarHeight`** _(Integer)_ - Minimum height of the input toolbar; default is `44`
- **`listViewProps`** _(Object)_ - Extra props to be passed to the messages [`<ListView>`](https://facebook.github.io/react-native/docs/listview.html); some props can't be overridden, see the code in `MessageContainer.render()` for details
- **`textInputProps`** _(Object)_ - Extra props to be passed to the [`<TextInput>`](https://facebook.github.io/react-native/docs/textinput.html)
- **`keyboardShouldPersistTaps`** _(Enum)_ - Determines whether the keyboard should stay visible after a tap; see [`<ScrollView>`](https://facebook.github.io/react-native/docs/scrollview.html) docs
- **`onInputTextChanged`** _(Function)_ - Callback when the input text changes
- **`maxInputLength`** _(Integer)_ - Max message composer TextInput length
- **`parsePatterns`** _(Function)_ - Custom parse patterns for [react-native-parsed-text](https://github.com/taskrabbit/react-native-parsed-text) used to linkify message content (like URLs and phone numbers), e.g.:
- **`extraData`** _(Object)_ - Extra props for re-rendering FlatList on demand. This will be useful for rendering footer etc.
- **`minComposerHeight`** _(Object)_ - Custom min height of the composer.
- **`maxComposerHeight`** _(Object)_ - Custom max height of the composer.


## License

- [MIT](LICENSE)

## Notes for local development
You need to have facebook watchman installed


1. `cd example`
2. `yarn`
4. `yarn start`
5. `yarn run sync` in another terminal window (doesn't matter where)

If you have any issues, you can clear your watches using `watchman watch-del-all` and try again.

## Author

Feel free to ask me question on Twitter [@JanRomaniak](https://www.twitter.com/JanRomaniak)!
