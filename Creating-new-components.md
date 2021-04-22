At OFN we now create components with `ViewComponent` (https://viewcomponent.org/) and we develop it with `Storybook` (https://storybook.js.org/). This wiki page is here to explain how it works, and what is the workflow of component creation.

### Create a component
Lots of convention and explanations are described here, on the official doc of ViewComponent: https://viewcomponent.org/guide/

Basically, we create component like this: 
```
rails generate component ComponentName title --sidecar
```
This will generate a component:
 - named `ComponentName`
 - with one attribute `title`
 - assets will be stored in a `component_name_component` folder.

This will create 3 files:
 - `app/components/component_name_component.rb`: the code of this component
 - `app/components/component_name_component/component_name_component.html.haml`: the template of this component
 - `spec/components/component_name_component_spec.rb`: the spec of this component

### Add story of this component in Storybook
#### Write a story
Stories must be placed into the `spec/components/stories` folder. 

Here's an example for the new component created with two stories one testing with short text, the second one testing with long text.
``` ruby
# spec/components/stories/component_name_component_stories.rb
class ComponentNameComponentStories < ViewComponent::Storybook::Stories
  story(:with_short_text) do
    controls do
      text(:title, 'OK')
    end
  end

  story(:with_long_text) do
    controls do
      text(:title, 'This is a long text')
    end
  end
end
```

You can find more example on how to write stories: https://github.com/jonspalmer/view_component_storybook#usage

#### Link this story with Storybook
We use https://github.com/jonspalmer/view_component_storybook to create links between ViewComponent and Storybook. 
`view_component_storybook` provide an helper to create this link. Simply run: 
```
rake view_component_storybook:write_stories_json
```
To create stories in json (which will be gitignored) that can be handled by Storybook.

#### Run Storybook and view your stories
Once stories are created and transformed into json, simple launch Storybook to view it:
```
yarn storybook
```
Enjoy! 🎉 

<img width="508" alt="Capture d’écran 2021-04-22 à 10 57 01" src="https://user-images.githubusercontent.com/296452/115686466-83961500-a359-11eb-9e08-bb280d72cbfc.png">

_NB_: The web application must be running to serve the assets and the components
