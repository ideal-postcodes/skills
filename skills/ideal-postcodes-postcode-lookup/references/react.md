# Add to React

1. Create a `context` container and attach a reference with `createRef`
2. Bind Postcode Lookup with `setup` passing in reference as context
3. Update address state using `onAddressSelected` callback.

```jsx
import "./styles.css";
import { useEffect, createRef, useState } from "react";
import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

const PostcodeLookupComponent = (props) => {
  const context = createRef();

  useEffect(() => {
    PostcodeLookup.setup({
      apiKey: "ak_test",
      context: context.current,
      buttonStyle: {
        backgroundColor: "green",
        color: "white",
      },
      ...props,
    });
  }, []);

  return <div ref={context}></div>;
};

export default function App() {
  const [address, setAddress] = useState({
    line_1: "",
    line_2: "",
    line_3: "",
    post_town: "",
    postcode: "",
  });

  return (
    <div className="App">
      <PostcodeLookupComponent
        onAddressSelected={(address) => setAddress(address)}
      />

      <label>Address Line One</label>
      <input
        type="text"
        value={address.line_1}
        onChange={(e) => setAddress({ ...address, line_1: e.target.value })}
      />
      <label>Address Line Two</label>
      <input
        type="text"
        value={address.line_2}
        onChange={(e) => setAddress({ ...address, line_2: e.target.value })}
      />
      <label>Address Line Three</label>
      <input
        type="text"
        value={address.line_3}
        onChange={(e) => setAddress({ ...address, line_3: e.target.value })}
      />
      <label>Post Town</label>
      <input
        type="text"
        value={address.post_town}
        onChange={(e) => setAddress({ ...address, post_town: e.target.value })}
      />
      <label>Postcode</label>
      <input
        type="text"
        value={address.postcode}
        onChange={(e) => setAddress({ ...address, postcode: e.target.value })}
      />
    </div>
  );
}
```
