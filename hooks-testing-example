import React from 'react';
import { Provider } from 'react-redux';
import { mount } from 'enzyme';
import configureMockStore from 'redux-mock-store';
import { findByDataAttribute } from '../../testsUtils/reusedFuntions';
import CreateEventForm from '../../components/BasicInfo/CreateEvent';
import createProductDefinitionEventActionCreator from '../../actions/productDefinitonEventActions';

const initialState = {};
const mockStore = configureMockStore(initialState);
const store = mockStore({ startup: { complete: false } });
const onSubmitFn = jest.fn();

const wrapperFn = props => mount(
  <Provider store={store}>
    <CreateEventForm {...props} onSubmit={onSubmitFn} />
  </Provider>,
);
const useStatespy = jest.spyOn(React, 'useState');

describe(' ==== testing Create Event component ======', () => {
  let component;
  let wrapper;
  const props = {};
  beforeEach(() => {
    wrapper = wrapperFn(props);
    component = findByDataAttribute(wrapper, 'create-event');
  });


  it(' the component is rendered proprely ', () => {
    expect(component.length).toEqual(1);
  });


  it(' the component has 1  link  field', () => {
    const link = findByDataAttribute(wrapper, 'create-event-link');
    expect(link.length).toBe(2);
  });
  it(' form  has 2  input-fields displayed after clicking the link ', () => {
    const link = findByDataAttribute(wrapper, 'create-event-link');
    link.at(0).simulate('click');
    expect(findByDataAttribute(wrapper, 'desc-input-field').length).toEqual(2);
    expect(findByDataAttribute(wrapper, 'name-input-field').length).toEqual(2);
  });
  it(' form  has 1  submit-field displayed after clicking the link ', () => {
    const link = findByDataAttribute(wrapper, 'create-event-link');
    link.at(0).simulate('click');
    expect(useStatespy).toBeCalled();
    expect(findByDataAttribute(wrapper, 'submit-field').length).toEqual(1);
  });

  it(' form  cant submit data  once the button was  clicked if inputs are empty', () => {
    const link = findByDataAttribute(wrapper, 'create-event-link');
    link.at(0).simulate('click');
    expect(useStatespy).toBeCalled();
    const submitButton = findByDataAttribute(wrapper, 'submit-field');
    const form = wrapper.find('form');
    expect(form.length).toEqual(1);
    expect(submitButton.length).toEqual(1);
    submitButton.simulate('click');
    expect(onSubmitFn).toHaveBeenCalledTimes(0);
  });
  it('the inputs are taking the changes', () => {
    const link = findByDataAttribute(wrapper, 'create-event-link');
    link.at(0).simulate('click');
    const nameInput = wrapper.find('[dataTest="name-input-field"]');
    const descInput = findByDataAttribute(wrapper, 'desc-input-field');
    expect(nameInput.length).toEqual(2);
    expect(descInput.length).toEqual(2);
    for (let i = 0; i < nameInput.length; i += 1) {
      if (nameInput[i]) {
        nameInput[i].props().onChange('test');
        descInput[i].props().onChange('test');
        expect(nameInput[i].props().value).toEqual('test');
        expect(descInput[i].props().value).toEqual('test');
      }
    }
  });

  it('the form is submitting inputs once the inputs exist ', () => {
    const link = findByDataAttribute(wrapper, 'create-event-link');
    link.at(0).simulate('click');
    const nameInput = wrapper.find('[dataTest="name-input-field"]');
    const descInput = findByDataAttribute(wrapper, 'desc-input-field');
    const submit = findByDataAttribute(wrapper, 'submit-field');
    for (let i = 0; i < nameInput.length; i += 1) {
      if (nameInput[i]) {
        nameInput[i].props().onChange('test');
        descInput[i].props().onChange('test');
        submit.simulate('click');
        expect(onSubmitFn).toBeCalled();
      }
    }
  });
  it('the form disapper once the cancel button is clicked', () => {
    const link = findByDataAttribute(wrapper, 'create-event-link');
    link.at(0).simulate('click');
    const cancelButton = wrapper.find('[dataTest="cancel-field"]');
    cancelButton.simulate('click');
    const descInput = findByDataAttribute(wrapper, 'desc-input-field');
    expect(descInput.length).toEqual(0);
    const submit = findByDataAttribute(wrapper, 'submit-field');
    expect(submit.length).toEqual(0);
    const nameInput = wrapper.find('[dataTest="name-input-field"]');
    expect(nameInput.length).toEqual(0);
  });

  it('once the add button clicked, an action is triggered ', () => {
    const reactRedux = jest.mock('react-redux', () => ({
      useDispatch: () => { },
      useSelector: () => {
      },
    }));

    const mockCreateProductDefinitionEventActionCreator = jest.fn(
      () => createProductDefinitionEventActionCreator(),
    );

    reactRedux.useDispatch = jest.fn(() => (cb) => { cb(); });
    const dispatch = reactRedux.useDispatch();


    const link = findByDataAttribute(wrapper, 'create-event-link');
    link.at(0).simulate('click');
    const nameInput = wrapper.find('[dataTest="name-input-field"]');
    const descInput = findByDataAttribute(wrapper, 'desc-input-field');
    const submit = findByDataAttribute(wrapper, 'submit-field');
    const form = wrapper.find('form');
    expect(form.length).toEqual(1);
    for (let i = 0; i < nameInput.length; i += 1) {
      if (nameInput[i]) {
        nameInput[i].props().onChange('test');
        descInput[i].props().onChange('test');
        form.props().onSubmit = jest.fn(
          () => dispatch(mockCreateProductDefinitionEventActionCreator),
        );
        submit.simulate('click');
        expect(onSubmitFn).toBeCalled();
        expect(mockCreateProductDefinitionEventActionCreator).toHaveBeenCalled();
      }
    }
  });
});
