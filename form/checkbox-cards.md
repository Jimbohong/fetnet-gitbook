# CheckboxCards

可選擇商品的牌卡

![CheckboxCards](../images/checkbox-cards.png)

## Usage
```jsx
  import CheckboxCards from '../components/form/CheckboxCards';

  constructor(props) {
    super(props);

    this.state = {
      products: [
        {
          type: 'product',
          link: '/estore/product/detail/iphone-11-pro-max-256gb',
          ribbon: '網路限定優惠',
          image: '/resources/cbu/find-product/estore-product-thumb-01@2x.jpg',
          tag: '5G手機',
          meta: 'APPLE',
          name: 'iPhone 11 Pro Max 256GB',
          salePrice: 1399,
          productPrice: 10500,
          storage: 3,
          value: '1',
        },
        ...
      ],
      form: {
        phone: { value: '' },
      },
    }
  }

  render() {
    return(
      <CheckboxCards
        name='phone'
        perPage={12}
        options={this.state.products}
        value={this.state.form.phone.value}
        withNumberController={true}
        onChange={(n, v, obj) => this.inputChange(n, v, obj)}
      />
    )
  }
```


## Sources

```jsx
import React from 'react';
import Slider from 'react-slick';
import PropTypes from 'prop-types';
import { withFormsy } from 'formsy-react';
import NumberSelector from './NumberSelector';
import LoadMore from '../LoadMore';
import formatNumber from '../../utils/numberFormatter';

class CheckboxCards extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      pages: 0,
      currentPage: 0,
      selected: this.props.value ? this.props.value.split(/,/g) : [],
      defaultChecked: this.props.value ? this.props.value.split(/,/g) : [],
      list: [],
      options: [],
    };
  }

  componentDidMount() {
    if (this.props.default) {
      this.props.setValue(this.props.value);
    }

    if (this.props.withNumberController) {
      this.setState({
        list: this.props.options.reduce((accr, val, idx, arr) => {
          accr.push({
            ...val,
            number: 1,
          });
          return accr;
        }, []),
      });

      this.forceUpdate();
    } else {
      this.setState({
        list: this.props.options,
      });
    }

    if (this.props.perPage) {
      this.setState({
        pages: Math.ceil(this.props.options.length / this.props.perPage),
      });

      setTimeout(() => {
        this.getMorePage();
      }, 300);
    } else {
      this.setState({
        options: this.props.list,
      });
    }
  }

  componentDidUpdate(prevProps) {
    if (prevProps.value !== this.props.value) {
      this.setState({
        defaultChecked: this.props.value ? this.props.value.split(/,/g) : [],
        selected: this.props.value ? this.props.value.split(/,/g) : [],
      });

      setTimeout(() => {
        this.getMorePage();
      }, 300);
    }

    if (prevProps.options.length !== this.props.options.length) {
      this.setState({
        list: this.props.options.reduce((accr, val, idx, arr) => {
          accr.push({
            ...val,
            number: 1,
          });
          return accr;
        }, []),
      });

      if (this.props.perPage) {
        this.setState({
          pages: Math.ceil(this.props.options.length / this.props.perPage),
          currentPage: 0,
        });

        setTimeout(() => {
          this.getMorePage();
        }, 300);
      } else {
        this.setState({
          list: this.props.options,
        });
      }

      this.forceUpdate();
    }
  }

  getMorePage = () => {
    this.setState({
      currentPage: this.state.currentPage + 1,
      options: [
        ...this.state.options,
        ...this.state.list.slice(
          this.state.currentPage * this.props.perPage,
          (this.state.currentPage + 1) * this.props.perPage
        ),
      ],
    });
  };

  numberChange = (i, v) => {
    let opts = Object.assign(this.state.options);
    opts[i].number = v;
    this.setState({
      options: opts,
    });

    let selected = Object.assign(this.state.selected);
    let objects = opts.reduce((accr, obj, index, array) => {
      if (selected.indexOf(obj.value) > -1) accr.push(obj);

      return accr;
    }, []);

    this.props.onChange(this.props.name, selected.toString(), objects);
  };

  handleChange = (event) => {
    let selected = Object.assign(this.state.selected);
    let val = event.currentTarget.value;
    if (event.currentTarget.checked) {
      selected.push(val);
    } else {
      selected.splice(selected.indexOf(val), 1);
    }
    // debugger
    this.setState({ selected: selected });
    this.props.setValue(selected.length ? selected.toString() : null);

    let objects = this.state.options.reduce((accr, obj, index, array) => {
      if (selected.indexOf(obj.value) > -1) accr.push(obj);

      return accr;
    }, []);

    this.props.onChange(this.props.name, selected.toString(), objects);
  };

  getDefaultCheck = (option) => {
    let isCheck = this.state.defaultChecked.indexOf(option.value) > -1;
    // console.log(this.state.defaultChecked, option, isCheck);
    return isCheck;
  };

  render() {
    return (
      <div className='fui-checkbox-cards-container'>
        <div className='fui-cards four-card no-scrollbar'>
          {this.state.list.map((opt, i) => (
            <div
              className={`fui-card is-${
                opt.type === 'product' ? 'product' : opt.type === 'wireless' ? 'wireless' : 'plan'
              } is-checkbox ${this.getDefaultCheck(opt) ? 'is-checked' : ''}`}
              key={`no-scrollbar-checkcard-${this.props.name}-${opt.value}`}>
              <label className='fui-card-action'>
                <input
                  type='checkbox'
                  name={this.props.name}
                  value={opt.value}
                  onChange={(e) => this.handleChange(e)}
                  checked={this.getDefaultCheck(opt)}
                />
                {opt.type === 'product' || opt.type === 'wireless' ? (
                  <div className='fui-card-image'>
                    <img
                      src={opt.image || process.env.PUBLIC_URL + '/resources/common/images/no-image@2x.png'}
                      alt={opt.title || opt.name}
                    />
                  </div>
                ) : null}
                {opt.type === 'friday' ? (
                  <div className='fui-card-plan is-friday'>
                    <div>
                      <h1>${formatNumber(opt.productPrice)}</h1>
                      <h4>{opt.meta}</h4>
                    </div>
                  </div>
                ) : null}
                {opt.type === 'music' ? (
                  <div className='fui-card-plan is-music'>
                    <div>
                      <h1>${formatNumber(opt.productPrice)}</h1>
                      <h4>{opt.meta}</h4>
                    </div>
                  </div>
                ) : null}

                <div className='fui-card-caption'>
                  {opt.type === 'product' ? <div className='fui-card-meta'>{opt.meta}</div> : null}
                  <div className='fui-card-title'>
                    {opt.tag ? <span className='tag'>{opt.tag}</span> : null}
                    {opt.name || opt.title}
                  </div>
                  <div className='fui-card-price'>
                    <div>
                      <span className='label'>{opt.type === 'product' ? '商品' : '加購'}價</span>
                      {opt.originPrice ? <del>${formatNumber(opt.originPrice)}</del> : null}
                      <b className={`price ${opt.originPrice ? 'is-text-error' : ''}`}>
                        ${formatNumber(opt.productPrice)}
                        {opt.day || opt.unit ? '/' : ''}
                        {opt.day || ''}
                        {opt.unit || ''}
                      </b>
                    </div>
                  </div>
                </div>
              </label>
              {opt.modal ? (
                <div className='fui-card-extra'>
                  <div role='button' className='is-text-underline' onClick={(e) => this.props.modalOpen(opt.modal)}>
                    規格說明
                  </div>
                </div>
              ) : null}
              {this.props.withNumberController ? (
                <NumberSelector
                  name={this.props.name + '-number'}
                  min={1}
                  value={opt.number}
                  onChange={(n, v) => this.numberChange(i, v)}
                />
              ) : null}
            </div>
          ))}
        </div>
        <LoadMore
          className='mb-0'
          moreLabel='看更多'
          noMoreLabel={'沒有更多商品'}
          load={this.state.currentPage < this.state.pages}
          click={this.getMorePage}
        />
      </div>
    );
  }
}

CheckboxCards.propTypes = {
  perPage: PropTypes.number,
  name: PropTypes.string,
  options: PropTypes.array,
  value: PropTypes.string,
  onChange: PropTypes.func,
  modalOpen: PropTypes.func,
  withNumberController: PropTypes.bool,
};

export default withFormsy(CheckboxCards);
```


### Properties
| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| name | String | true |  | 輸入框名稱 |
| value | String | true |  | 值 |
| perPage | Boolean |  |  | 預設顯示數量，超過則點選「看更多」顯示 |
| options | Array |  |  | options 細節參照使用方式 |
| withNumberController | Boolean |  |  | 牌卡是否能選擇數量 |
| onChange | Function |  |  | 選取或數量更新異動回傳 |
| modalOpen | String |  |  | 傳入跳窗內容與屬性 |

