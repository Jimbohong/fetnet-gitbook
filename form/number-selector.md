# NumberSelector
數量選擇器大多使用在牌卡與購物車中，也可單獨使用

## Usage
```jsx
import NumberSelector from '../../components/form/NumberSelector';

class Product extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      form: {
        number: { value: 1, required: true },
      }
    }
  }

  render() {
    return (
      <NumberSelector
        name='number'
        value={form.number.value}
        min={1}
        max={3}
        onChange={this.inputChange}
      />
    )
  }
}
```

## Sources
```jsx
import React from 'react';
import PropTypes from 'prop-types';

class NumberSelector extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      number: this.props.value,
      showNote: false,
    };
  }

  componentDidUpdate(prevProps, prevState) {
    if (prevProps.value !== this.props.value || prevProps.max !== this.props.max) {
      this.setState({
        number: this.props.value,
        showNote: this.props.value + 1 > this.props.max && this.props.max !== -1,
      });
      this.forceUpdate();
    }
  }

  decrease = () => {
    if (this.state.number - 1 >= this.props.min) this.props.onChange(this.props.name, this.state.number - 1);
  };

  increase = () => {
    if (
      !this.props.max ||
      this.props.max === -1 ||
      (this.props.max && this.props.max !== -1 && this.state.number + 1 <= this.props.max)
    ) {
      this.props.onChange(this.props.name, this.state.number + 1);
    }
  };

  render() {
    return (
      <div className='fui-number-selector-container'>
        <div className='fui-number-selector'>
          <div role='button' onClick={this.decrease} className={this.props.min === this.state.number ? 'disabled' : ''}>
            <i className='icon-minus'></i>
          </div>
          <div>
            <input
              type='input'
              name={this.props.name}
              value={this.state.number}
              min={this.props.min}
              max={this.props.max}
              readOnly={true}
            />
          </div>
          <div role='button' onClick={this.increase} className={this.props.max === this.state.number ? 'disabled' : ''}>
            <i className='icon-plus'></i>
          </div>
        </div>
        {this.state.showNote ? <div className='is-text-error'>{this.props.maxMessage || '已達購買上限'}</div> : null}
      </div>
    );
  }
}

NumberSelector.propTypes = {
  name: PropTypes.string,
  value: PropTypes.number,
  min: PropTypes.number,
  max: PropTypes.number,
  maxMessage: PropTypes.string,
  onChange: PropTypes.func,
};

export default NumberSelector;
```

## Properties
| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| name | String | true |  | 欄位名稱 |
| value | String | true |  | 說明文字 |
| min | Number |  |  | 最少數量，不填則預設為 1 |
| max | Number |  |  | 最大數量，不填則預設為 -1，可無限選擇 |
| maxMessage | String |  |  | 到最大值的提示訊息 |
| onChange | Function |  |  | 數量變更後的回傳 |