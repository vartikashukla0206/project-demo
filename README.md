# project-demo
This is my first Git Repository
<br>
Author- Vartika Shukla
import { MdDeleteForever } from "react-icons/md";
import {
  addDetailRec,
  detailHandleChangeR,
  handleChangeR,
  removeDetailRec,
} from "../../slices/formSlice";
import { useDispatch, useSelector } from "react-redux";
import { useState, useRef, useEffect } from "react";
import HelpList from "../Form_Emp_salary_detail/HelpList";



const EarnDeductMaster = () => {
  const dispatch = useDispatch();

  const EarnDeductMaster = useSelector(
    (store) => store.formData.EarnDeductMaster
  );
  const mdpmpay_factor = useSelector(
    (store) => store.formData.EarnDeductMaster.mdpmpay_factor
  );

  const [isHelpShown, setIsHelpShown] = useState(false);
  const [currIndex, setCurrIndex] = useState("");
  const [dataList, setDataList] = useState([]);
  const [dataType, setDataType] = useState("");

  
  const inputRefs = Array.from({ length: 20 }, () => useRef());

  //-----------------------------Header Block Section ---------------------------------

  // for header block input changes
  const handleChange = (e) => {
    const { name, value } = e.target;
    const newValue = value.toUpperCase();
    dispatch(handleChangeR({ [name]: newValue }));
  };

  //for header block input type checkbox
  const handleCheckBox = (e, data) => {
    const { name } = e.target;
    if (data) {
      dispatch(handleChangeR({ [name]: "Y" }));
    } else {
      dispatch(handleChangeR({ [name]: "N" }));
    }
  };

  //---------------------------Detail Block Section-------------------------------------

  //for adding Detail record
  const addNewRecord = () => {
    const newRecord = {
      category: "",
      grade: "",
    };
    dispatch(addDetailRec([...mdpmpay_factor, newRecord]));
  };

  // delete the record of detail section
  const removeRecordField = (index) => {
    let preRecord = [...mdpmpay_factor];
    preRecord.splice(index, 1);
    dispatch(removeDetailRec(preRecord));
  };

  //detail section input change
  const detailHandleChange = (event, index) => {
    const { name, value, checked, type } = event.target;

    let newValue = value;
    if (type === "checkbox") {
      newValue = checked ? "Y" : "N";
    } else if (type === "radio") {
      newValue = value;
      const radioData = mdpmpay_factor.map((item, idx) =>
        idx === index ? { ...item, ["rupees_paise"]: newValue } : item
      );
      dispatch(detailHandleChangeR(radioData));
      return;
    } else if (type === "select-one") {
      // Check if the input type is a select element
      newValue = value; // Get the value of the selected option directly
    } else {
      newValue = value.toUpperCase();
    }

    const updatedData = mdpmpay_factor.map((item, idx) =>
      idx === index ? { ...item, [name]: newValue } : item
    );
    dispatch(detailHandleChangeR(updatedData));
  };

  // when user want to fill the mdpmpay_factor/Details input by help the help data come according to existing header detail
  const openHelpBox = async (query, index) => {
    // console.log(category, grade, index);
    setCurrIndex(index);

    const res = await fetch(
      `http://localhost:3080/api/Earn_Deduct/help_list/${query}`,
      {
        method: "GET",
        // headers: {
        //   "Content-Type": "application/json",
        // },
        // body: JSON.stringify({ p_categroy: category, p_grade: grade }),
      }
    );
    if (res.ok) {
      const result = await res.json();
      console.log(result);
      setDataList(result);
      setDataType(query);
      setIsHelpShown(true);
    } else {
      alert("something wrong in fetching data from the database");
    }
  };

  //this function work by helpList component as lisfting state
  const handleHelpState = (data) => {
    setIsHelpShown(data);
  };




  useEffect(()=>{
    inputRefs[0].current.focus()
  },[])

  return (
    <div className="h-[80vh] w-full  p-2">
      <div className="px-3 p-1 w-full h-full ">
        <section className=" w-full max-h-[24vh] flex justify-center items-center">
          <div className="w-full rounded-md h-full border border-slate-300 max-w-[1180px] py-1 pt-3 relative">
            <div className="w-full  flex justify-normal items-center">
              <div className="  lg:w-[80%]  ">
                <div className="flex py-[2px]  md:w-full ">
                  <div className="w-1/2 flex justify-normal items-end ">
                    <label className="w-[25%] text-right px-3 text-sm  flex justify-end items-center">
                      Description
                    </label>
                    <input
                      type="text"
                      ref={inputRefs[0]}
                      name="ed_desc"
                      autoComplete="off"
                      value={EarnDeductMaster.ed_desc || ""}
                      className=" w-[75%] ml-2 border-slate-400 rounded-[4px] border outline-none h-6 px-2 text-sm md:text-md"
                      onDoubleClick={() => {
                        //   openHelpBox("mopmmonths");
                      }}
                      onChange={handleChange}
                      onKeyDown={(e)=>{
                       if(e.key === 'Enter'|| e.key === 'Tab'){
                        inputRefs[1].current.focus()
                       }
                      }
                      }
                    />
                  </div>
                  <div className="w-1/2 px-4 flex justify-normal items-end ">
                    <label className="w-[17%] px-3 text-sm text-end ">
                      Code
                    </label>
                    <input
                      type="text"
                      name="ed_code"
                      ref={inputRefs[1]}
                      autoComplete="off"
                      value={EarnDeductMaster.ed_code || ""}
                      onChange={handleChange}
                      onKeyDown={(e)=>{
                        if(e.key === 'Enter'|| e.key === 'Tab'){
                         inputRefs[2].current.focus()
                        }
                       }
                       }
                      
                      className="border-slate-400 rounded-[4px] border outline-none h-6 px-2 text-sm w-[30%]"
                    />
                  </div>
                </div>
                <div className=" w-full flex ">
                  <div className="w-1/2 flex justify-normal items-center ">
                    <label className="w-[25%] text-right px-3 text-sm flex justify-end items-center">
                      Short Desc.
                    </label>
                    <input
                      type="text"
                      name="ed_short_desc"
                      ref={inputRefs[2]}
                      autoComplete="off"
                      value={EarnDeductMaster.ed_short_desc || ""}
                      onChange={handleChange}
                      onKeyDown={(e)=>{
                        if(e.key === 'Enter'|| e.key === 'Tab'){
                         inputRefs[3].current.focus()
                        }
                       }
                       }
                
                      className=" w-[75%] ml-2 border-slate-400 rounded-[4px] border outline-none h-6 px-2 text-sm md:text-md"
                    />
                  </div>
                  <div className="flex justify-start items-center py-[2px]  w-[40%]">
                    <label className="text-sm md:text-md flex justify-end items-center  pl-8">
                      Acc. No.
                    </label>
                    <input
                      type="text"
                      name="acc_no"
                      ref={inputRefs[3]}
                      value={EarnDeductMaster.acc_no || ""}
                      autoComplete="off"
                      className="w-[10%] sm:w-[15%] ml-2 border-slate-400 rounded-[4px] border outline-none h-6 px-2 text-sm md:text-md  md:w-[12%]"
                      onChange={handleChange}
                      onKeyDown={(e)=>{
                        if(e.key === 'Enter'|| e.key === 'Tab'){
                         inputRefs[5].current.focus()
                        }
                       }
                       }
                    />
                    <input
                      type="text"
                      autoComplete="off"
                      ref={inputRefs[4]}
                      disabled={true}
                      className="w-[40%] sm:w-[50%] ml-2 border-slate-400 rounded-[4px] border outline-none h-6 px-2 text-sm md:text-md  md:w-[60%] lg:w-[60%]"
                      onKeyDown={(e)=>{
                        if(e.key === 'Enter'|| e.key === 'Tab'){
                         inputRefs[5].current.focus()
                        }
                       }
                       }
                    />
                  </div>
                </div>
              </div>
              <div className="w-[20%]">
                <div className="border-slate-400 border relative rounded-lg">
                  <div className="w-full">
                    <div className="flex justify-between items-center pt-3 w-full">
                      <label className="items-center w-1/2  pl-2">
                        <input
                          type="radio"
                          name="ed_type"
                          className="mr-1 "
                          ref={inputRefs[5]}
                          checked={
                            EarnDeductMaster.ed_type === "E" ? true : false
                          }
                          onChange={(e) => {
                            const { name } = e.target;
                            dispatch(handleChangeR({ [name]: "E" }));
                          }}
                          onKeyDown={(e)=>{
                            if(e.key === 'Enter'|| e.key === 'Tab'){
                             inputRefs[6].current.focus()
                            }
                           }
                           }
                        />
                        Earning
                      </label>
                      <label className="flex justify-center w-1/2 ">
                        <input
                          type="radio"
                          name="ed_type"
                          className="mr-1"
                          checked={
                            EarnDeductMaster.ed_type === "D" ? true : false
                          }
                          ref={inputRefs[6]}
                          onChange={(e) => {
                            const { name } = e.target;
                            dispatch(handleChangeR({ [name]: "D" }));
                          }}
                          onKeyDown={(e)=>{
                            if(e.key === 'Enter'|| e.key === 'Tab'){
                             inputRefs[7].current.focus()
                            }
                           }
                           }
                        />
                        Deduction
                      </label>
                    </div>
                  </div>
                  <p className="absolute top-[-0.5rem]  rounded-lg px-3 left-3  bg-indigo-500 text-white  text-sm">
                    Type
                  </p>
                </div>
              </div>
            </div>

            <div className="w-full flex justify-normal items-center  ">
              <div className="flex justify-start items-center py-[2px] w-[40%]">
                <label className="text-sm md:text-md w-[25%] flex justify-end items-center px-3 ">
                  Transaction
                </label>
                <select
                  className=" w-[50%] sm:w-[60%] ml-2 border-slate-400 rounded-[4px] border outline-none h-6 px-2 text-sm md:text-md  md:w-[50%]"
                  name="Trans_flag"
                  ref={inputRefs[7]}
                  onKeyDown={(e)=>{
                    if(e.key === 'Enter'|| e.key === 'Tab'){
                     inputRefs[8].current.focus()
                    }
                   }
                   }
                >
                  <option value="">--select--</option>
                  <option value="1">Transaction</option>
                  <option value="2">option</option>
                </select>
              </div>
              <div className="flex  justify-evenly w-[60%] items-center">
                <label className="sm:ml-2 md:ml-5 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    name="hra_exempt_itax"
                    checked={
                      EarnDeductMaster.hra_exempt_itax === "Y" ? true : false
                    }
                    ref={inputRefs[8]}
                    onChange={(e) => {
                      handleCheckBox(e, e.target.checked);
                    }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[9].current.focus()
                      }
                     }
                     }
                  />
                  For HRA Calculation
                </label>
                <label className="sm:ml-2 md:ml-5 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    name="lease_exempt_itax"
                    ref={inputRefs[9]}
                    checked={EarnDeductMaster.lease_exempt_itax === "Y" ? true : false}
                    onChange={(e) => {
                      handleCheckBox(e, e.target.checked);
                    }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[10].current.focus()
                      }
                     }
                     }
                  />
                  For Lease Calculation
                </label>
                <label className="sm:ml-2 md:ml-5 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    name="inc_in_proj_itax"
                    ref={inputRefs[10]}
                    checked={
                      EarnDeductMaster.inc_in_proj_itax === "Y" ? true : false
                    }
                    onChange={(e) => {
                      handleCheckBox(e, e.target.checked);
                    }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[11].current.focus()
                      }
                     }
                     }
                  />
                  Include in Itax Projection
                </label>
              </div>
            </div>
            <div className="w-full md:px-2 flex justify-start p-1">
              <div className="flex sm:w-full md:w-full lg:w-[78%] justify-evenly items-center border py-1">
                <label className="sm:ml-2 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    name="vp_empwise"
                    ref={inputRefs[11]}
                    checked={EarnDeductMaster.vp_empwise === "Y" ? true : false}
                    onChange={(e) => {
                      handleCheckBox(e, e.target.checked);
                    }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[12].current.focus()
                      }
                     }
                     }
                  />
                  Employee Wise
                </label>
                <label className="sm:ml-2 md:ml-5 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    name="one_time"
                    ref={inputRefs[12]}
                    checked={EarnDeductMaster.one_time === "Y" ? true : false}
                    onChange={(e) => {
                      handleCheckBox(e, e.target.checked);
                    }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[13].current.focus()
                      }
                     }
                     }
                  />
                  One Time
                </label>
                <label className="sm:ml-2 md:ml-5 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    ref={inputRefs[13]}
                    // name="suspended_flag"
                    // checked={EarnDeductMaster.one_time === "Y" ? true : false}
                    // onChange={(e) => {
                    //   handleCheckBox(e, e.target.checked);
                    // }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[14].current.focus()
                      }
                     }
                     }
                  />
                  Taxable
                </label>
                <label className="sm:ml-2 md:ml-5 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    name="dont_cf_suppl"
                    ref={inputRefs[14]}
                    checked={EarnDeductMaster.dont_cf_suppl === "Y" ? true : false}
                    onChange={(e) => {
                      handleCheckBox(e, e.target.checked);
                    }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[15].current.focus()
                      }
                     }
                     }
                  />
                  Supp
                </label>
                <label className="sm:ml-2 md:ml-5 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    name="arrears"
                    ref={inputRefs[15]}
                    checked={EarnDeductMaster.arrears === "Y" ? true : false}
                    onChange={(e) => {
                      handleCheckBox(e, e.target.checked);
                    }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[16].current.focus()
                      }
                     }
                     }
                  />
                  Arrears
                </label>
                <label className="sm:ml-2 md:ml-5 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    name="inc_in_salary"
                    ref={inputRefs[16]}
                    checked={
                      EarnDeductMaster.inc_in_salary === "Y" ? true : false
                    }
                    onChange={(e) => {
                      handleCheckBox(e, e.target.checked);
                    }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[17].current.focus()
                      }
                     }
                     }
        
                     
                  />
                  Include In Salary
                </label>
                <label className="sm:ml-2 md:ml-5 cursor-pointer text-sm">
                  <input
                    type="checkbox"
                    className="mr-2"
                    name="ded_flag"
                    ref={inputRefs[17]}
                    checked={EarnDeductMaster.ded_flag === "Y" ? true : false}
                    onChange={(e) => {
                      handleCheckBox(e, e.target.checked);
                    }}
                    onKeyDown={(e)=>{
                      if(e.key === 'Enter'|| e.key === 'Tab'){
                       inputRefs[18].current.focus()
                      }
                     }
                     }
                  />
                  Deduction Report Flag
                </label>
              </div>
            </div>
            <p className="absolute top-[-0.5rem]  rounded-lg px-2 left-3  bg-indigo-500 text-white  text-sm">
              Pay Factor Detail
            </p>
          </div>
        </section>
        {/* mt-2 will effect the form height in future in below section pls correct */}
        <section className="w-full flex flex-col justify-center items-center mt-2 h-[54vh] relative ">
          <div className="w-full rounded-md h-[85%] border border-slate-300 max-w-[1180px] py-1 flex flex-col relative ">
            <div className="border p-1 px-3 flex shadow-xl bg-slate-200 w-full h-[15%]">
              <div className="text-gray-700 font-bold md:w-[16%] border border-r-black flex justify-center text-sm">
                Category
              </div>
              <div className="text-gray-700 font-bold md:w-[8%] border border-r-black flex justify-center text-sm">
                Grade
              </div>
              <div className="text-gray-700 font-bold md:w-[8%] border border-r-black flex justify-center text-sm"></div>
              <div className="text-gray-700 font-bold lg:w-[4%] border border-r-black flex justify-center text-sm">
                Y/N
              </div>
              <div className="text-gray-700 font-bold md:w-[10%] border border-r-black flex justify-center text-sm">
                Rupees/paise
              </div>
              <div className="text-gray-700 font-bold lg:w-[6%] border border-r-black flex justify-center text-sm">
                Upto
              </div>
              <div className="text-gray-700 font-bold lg:w-[7%] border border-r-black flex justify-center text-sm">
                Direction
              </div>
              <div className="text-gray-700 font-bold lg:w-[5%] border border-r-black flex justify-center text-sm">
                %tage
              </div>
              <div className="text-gray-700 font-bold lg:w-[8%] border border-r-black flex justify-center text-sm">
                Incl Payslip
              </div>
              <div className="text-gray-700 font-bold  lg:w-[5%] border border-r-black flex justify-center text-sm">
                Active
              </div>
              <div className="text-gray-700 font-bold lg:w-[7%] border border-r-black flex justify-center text-sm">
                Bsd Atnd
              </div>
              <div className="text-gray-700 font-bold  lg:w-[10%] flex justify-center border border-r-black text-sm">
                Inc in sal Head
              </div>
              <div className="text-gray-700 font-bold lg:w-[7%] flex justify-center text-sm">
                Wrk Days
              </div>
            </div>
            <div className="w-full flex flex-col gap-1 pt-3 h-[85%] overflow-y-auto ">
              {mdpmpay_factor.map((form, index) => (
                <div className=" w-full" key={index}>
                  <div className="flex h-[25px] rounded-md">
                    <div className="border-r-slate-300 border w-[16.5%] h-full border-slate-300 rounded-l-md">
                      <input
                        autoComplete="off"
                        type="text"
                        name="cat_desc"
                        ref={inputRefs[18]}
                        value={form.cat_desc || ""}
                        onChange={(e) => {
                          detailHandleChange(e, index);
                        }}
                        onDoubleClick={(e) => {
                          openHelpBox("vwempcat", index);
                          console.log("click");
                        }}
                        className="w-full border-none outline-none px-2 h-full bg-sky-200 text-sm"
                      />
                    </div>
                    <div className="border-r-slate-300 border w-[8%] h-full border-slate-300 ">
                      <input
                        type="text"
                        className="w-full border-none outline-none px-2 h-full bg-sky-200 text-sm"
                        name="grade_desc"
                        autoComplete="off"
                        value={form.grade_desc || ""}
                        onChange={(e) => {
                          detailHandleChange(e, index);
                        }}
                        onDoubleClick={(e) => {
                          openHelpBox("vwempgrd", index);
                        }}
                      />
                    </div>
                    <div className="flex justify-start items-center py-[2px] w-[8%] border">
                      <select
                        className="outline-none border bg-sky-200 text-sm w-full"
                        name="ed_factor"
                        value={form.ed_factor || ""}
                        onChange={(e) => {detailHandleChange(e, index)}}
                        // value={Emp_salary_form.pf_to || ""}
                        // onChange={handleChange}
                      >
                        <option value="" className="text-sm">
                          --select--
                        </option>
                        <option value="A" className="text-sm">
                          Annum
                        </option>
                        <option value="M" className="text-sm">
                          Month
                        </option>
                        <option value="W" className="text-sm">
                          Week
                        </option>
                        <option value="D" className="text-sm">
                          Day
                        </option>
                        <option value="H" className="text-sm">
                          Hour
                        </option>
                      </select>
                    </div>
                    <div className=" w-[4%] flex justify-center items-center h-full border">
                      <input
                        type="checkbox"
                        name="rounded_off"
                        checked={form.rounded_off === "Y" ? true : false}
                        className=" w-4 h-full border border-slate-300 text-sm"
                        onChange={(e) => {
                          detailHandleChange(e, index);
                        }}
                      />
                    </div>
                    <div className="flex justify-evenly items-center w-[9.5%] border">
                      <label className="items-center w-1/2">
                        <input
                          type="radio"
                          name={`rupees_paise/${index}`}
                          className="mr-1 "
                          value="R"
                          checked={form.rupees_paise === "R"}
                          onChange={(e) => {
                            detailHandleChange(e, index);
                          }}
                        />
                        Rs.
                      </label>
                      <label className="flex justify-center w-1/2">
                        <input
                          type="radio"
                          name={`rupees_paise/${index}`}
                          value="P"
                          className="mr-1"
                          checked={form.rupees_paise === "P"}
                          onChange={(e) => {
                            detailHandleChange(e, index);
                          }}
                        />
                        Ps.
                      </label>
                    </div>
                    <div className="flex justify-start items-center py-[2px] w-[6%]">
                      <select
                        className="outline-none border bg-sky-200 w-full text-sm"
                        name="rounded_upto"
                        value={form.rounded_upto || ""}
                        onChange={(e) => {detailHandleChange(e,index)}}
                      >
                        <option value="" className="text-sm">
                          Upto
                        </option>
                        <option value="1" className="text-sm">
                          1
                        </option>
                      </select>
                    </div>
                    <div className="flex justify-start items-center py-[2px] w-[7%] border">
                      <select
                        className="outline-none border bg-sky-200 w-full text-sm"
                        name="round_direction"
                        value={form.round_direction || ""}
                        onChange={(e) => {detailHandleChange(e, index)}}
                      >
                        <option value="" className="text-sm">
                          -Dir-
                        </option>
                        <option value="U" className="text-sm">
                          Upper
                        </option>
                        <option value="L" className="text-sm">
                          Lower
                        </option>
                        <option value="N" className="text-sm">
                          Near
                        </option>
                      </select>
                    </div>
                    <div className="border-r-white border w-[5%] h-full border-slate-300 ">
                      <input
                        type="text"
                        className="w-full border-none outline-none px-2 h-full bg-sky-200"
                        autoComplete="off"
                        name="factor_perc"
                      />
                    </div>

                    <div className=" w-[6%] flex justify-center items-center h-full ">
                      <input
                        type="checkbox"
                        name="inc_in_payslip"
                        checked={form.inc_in_payslip === "Y" ? true : false}
                        className=" w-4 h-full border border-slate-300 text-sm"
                        onChange={(e) => {
                          detailHandleChange(e, index);
                        }}
                      />
                    </div>
                    <div className=" w-[5%] flex justify-end items-center h-full ">
                      <input
                        type="checkbox"
                        name="active"
                        checked={form.active === "Y" ? true : false}
                        className=" w-4 h-full border border-slate-300"
                        onChange={(e) => {
                          detailHandleChange(e, index);
                        }}
                      />
                    </div>
                    <div className=" w-[7%] flex justify-end items-center h-full px-2 ">
                      <input
                        type="checkbox"
                        name="based_on_atnd"
                        checked={form.based_on_atnd === "Y" ? true : false}
                        className=" w-4 h-full border border-slate-300"
                        onChange={(e) => {
                          detailHandleChange(e, index);
                        }}
                      />
                    </div>
                    <div className=" w-[10%] flex justify-center items-center h-full ">
                      <input
                        type="checkbox"
                        name="inc_in_salhead"
                        checked={form.inc_in_salhead === "Y" ? true : false}
                        className=" w-4 h-full border border-slate-300"
                        onChange={(e) => {
                          detailHandleChange(e, index);
                        }}
                      />
                    </div>
                    <div className=" w-[7%] flex justify-center items-center h-full ">
                      <input
                        type="checkbox"
                        name="working_days"
                        checked={form.working_days === "Y" ? true : false}
                        className=" w-4 h-full border border-slate-300"
                        onChange={(e) => {
                          detailHandleChange(e, index);
                        }}
                      />
                    </div>
                    <div className=" flex justify-evenly items-center w-[2%] h-full">
                      <MdDeleteForever
                        className=" w-full h-full text-black cursor-pointer hover:bg-gray-800 hover:text-white rounded-md duration-300 ease-in-out"
                        onClick={(e) => {
                          removeRecordField(index);
                        }}
                      />
                    </div>
                  </div>
                </div>
              ))}
            </div>
          </div>
          <div className=" w-full flex justify-end items-end rounded-lg max-w-[1180px] mt-2">
            <button
              className="w-[7%] rounded-lg bg-indigo-500 text-white hover:bg-indigo-700 h-full py-[2px]"
              onClick={addNewRecord}
            >
              Add +
            </button>
          </div>
        </section>
      </div>
      {isHelpShown === true ? (
        <div className="absolute top-[2rem] left-[2rem] w-full">
          <HelpList
            handleHelpState={handleHelpState}
            dataList={dataList}
            dataType={dataType}
            currIndex={currIndex}
          />
        </div>
      ) : (
        ""
      )}
    </div>
  );
};

export default EarnDeductMaster;
